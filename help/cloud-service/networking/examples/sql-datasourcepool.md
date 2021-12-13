---
title: Connexions SQL à l’aide de JDBC DataSourcePool
description: Découvrez comment vous connecter à des bases de données SQL à partir d’AEM as a Cloud Service à l’aide d’AEM JDBC DataSourcePool et des ports de sortie.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# Connexions SQL à l’aide de JDBC DataSourcePool

Les connexions à des bases de données SQL (et à d’autres services non HTTP/HTTPS) doivent être traitées par proxy hors AEM, y compris celles effectuées à l’aide du service OSGi DataSourcePool pour gérer les connexions.

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancées suivantes.

| Pas de mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP sortante dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuration OSGi

La chaîne de connexion de la configuration OSGi utilise :

+ `AEM_PROXY_HOST` via la fonction [Variable d’environnement de configuration OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` comme hôte de la connexion
+ `30001` qui correspond à la variable `portOrig` valeur du mappage de transfert de port Cloud Manager `30001` → `mysql.example.com:3306`

Puisque les secrets ne doivent pas être stockés dans le code, le nom d’utilisateur et le mot de passe de la connexion SQL sont mieux fournis via les variables de configuration OSGi, définies à l’aide de l’interface de ligne de commande AIO ou des API Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

Les éléments suivants `aio CLI` peut être utilisée pour définir les secrets OSGi par environnement :

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Exemple de code

Cet exemple de code Java™ illustre un service OSGi qui établit une connexion à une base de données MySQL externe via AEM service OSGi DataSourcePool.
La configuration d’usine OSGi DataSourcePool spécifie à son tour un port (`30001`) qui est mappé via le `portForwards` dans la [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) l&#39;opération sur l&#39;hôte externe et le port, `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
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
