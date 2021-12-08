---
title: Connexions SQL à l’aide des API Java™
description: Découvrez comment vous connecter à des bases de données SQL à partir d’AEM as a Cloud Service à l’aide des API Java™ SQL et des ports de sortie.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9356
thumbnail: KT-9356.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 0%

---


# Connexions SQL à l’aide des API Java™

Les connexions aux bases de données SQL (et autres services non HTTP/HTTPS) doivent être traitées par proxy hors AEM.

L’exception à cette règle est lorsque [adresse ip de sortie dédiée](../dedicated-egress-ip-address.md) est en cours d’utilisation et le service est sur Adobe ou Azure.

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancées suivantes.

| Pas de mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP sortante dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuration OSGi

Puisque les secrets ne doivent pas être stockés dans le code, le nom d’utilisateur et le mot de passe de la connexion SQL sont mieux fournis via [variables de configuration OSGi secrètes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), défini à l’aide de l’interface de ligne de commande AIO ou des API Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

Les éléments suivants `aio CLI` peut être utilisée pour définir les secrets OSGi par environnement :

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Exemple de code

Cet exemple de code Java™ illustre un service OSGi qui établit une connexion à un serveur Web de serveur SQL externe, au moyen de l’outil Cloud Manager suivant : `portForwards` de la règle [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) opération.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv("AEM_PROXY_HOST") != null ?
            System.getenv("AEM_PROXY_HOST") : "localhost";

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = System.getenv("AEM_PROXY_HOST") != null ? 30001 : 3306;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
    }
}
```

## Dépendances des pilotes MySQL

AEM as a Cloud Service nécessite souvent que vous fournissiez des pilotes de base de données Java™ pour prendre en charge les connexions. Il est généralement préférable de fournir les pilotes en incorporant les artefacts de bundle OSGi contenant ces pilotes dans le projet AEM via le `all` module.

### Reactor pom.xml

Inclure les dépendances du pilote de base de données dans le réacteur `pom.xml` puis référencez-les dans le `all` sous-projets.

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## Tout pom.xml

Incorporation des artefacts de dépendance de pilote de base de données dans `all` le module vers lequel ils sont déployés est disponible sur AEM as a Cloud Service. Ces artefacts __must__ être des lots OSGi qui exportent la classe Java™ du pilote de base de données.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
