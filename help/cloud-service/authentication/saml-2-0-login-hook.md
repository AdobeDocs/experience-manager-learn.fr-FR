---
title: Crochet de connexion personnalisé SAML 2.0
description: Découvrez comment développer un hook de connexion SAML 2.0 personnalisé pour AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# hook de connexion SAML 2.0

Découvrez comment développer un hook de connexion SAML 2.0 personnalisé pour AEM. Ce tutoriel fournit des instructions détaillées sur la création d’un hook de connexion personnalisé qui s’intègre à un fournisseur d’identité SAML 2.0 et permet aux utilisateurs de s’authentifier à l’aide de leurs identifiants SAML.

Si le FI ne peut pas envoyer les données de profil utilisateur et l’appartenance au groupe d’utilisateurs dans l’assertion SAML, ou si les données doivent être transformées avant la synchronisation vers AEM, des hooks SAML personnalisés peuvent être implémentés pour étendre le processus d’authentification SAML. Les hooks SAML permettent de personnaliser l’affectation des appartenances à un groupe, de modifier les attributs de profil utilisateur et d’ajouter une logique métier personnalisée pendant le flux d’authentification.

>[!NOTE]
>Les hooks SAML personnalisés sont pris en charge sur **AEM as a Cloud Service** et **AEM LTS**. Cette fonctionnalité n’est pas disponible sur les anciennes versions d’AEM.

## Cas d’utilisation courants :

Les hooks SAML personnalisés sont utiles lorsqu’il est nécessaire de :

+ Attribuez de manière dynamique l’appartenance à un groupe en fonction d’une logique commerciale personnalisée au-delà de ce qui est fourni dans les assertions SAML
+ Transformation ou enrichissement des données de profil utilisateur avant leur synchronisation avec AEM
+ Mappage de structures d’attributs SAML complexes avec les propriétés utilisateur AEM
+ Implémentation de règles d’autorisation personnalisées ou d’affectations de groupes conditionnels
+ Ajout d’une journalisation ou d’un audit personnalisé lors de l’authentification SAML
+ Intégration de systèmes externes pendant le processus d’authentification

## Interface du service OSGi `SamlHook`

L’interface `com.adobe.granite.auth.saml.spi.SamlHook` fournit deux méthodes de hook appelées à différentes étapes du processus d’authentification SAML :

### méthode `postSamlValidationProcess()`

Cette méthode est appelée **après** la validation de la réponse SAML, mais **avant** le démarrage du processus de synchronisation de l’utilisateur. Il s’agit de l’emplacement idéal pour modifier les données d’assertion SAML, telles que l’ajout ou la transformation d’attributs.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### Cas d’utilisation

+ Ajout d’appartenances de groupe supplémentaires à l’assertion
+ Transformation des valeurs d’attribut avant leur synchronisation
+ Enrichir l&#39;assertion avec des données provenant de sources externes
+ Valider les règles métier personnalisées

### méthode `postSyncUserProcess()`

Cette méthode est appelée **après** la fin du processus de synchronisation des utilisateurs. Ce hook peut être utilisé pour effectuer des opérations supplémentaires après la création ou la mise à jour de l’utilisateur AEM.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### Cas d’utilisation

+ Mettre à jour des propriétés de profil utilisateur supplémentaires non couvertes par la synchronisation standard
+ Créer ou mettre à jour des ressources personnalisées liées aux utilisateurs dans AEM
+ Déclencher des workflows ou des notifications après l’authentification de l’utilisateur
+ Consigner les événements d’authentification personnalisés

**Important :** pour modifier les propriétés de l’utilisateur dans le référentiel, l’implémentation du hook requiert les éléments suivants :

+ Une référence `SlingRepository` injectée via `@Reference`
+ Un [utilisateur de service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) configuré avec les autorisations appropriées (configuré dans « Apache Sling Service User Mapper Service Amendment »)
+ Gestion appropriée des sessions avec des blocs try-catch-finish

## Implémenter un hook SAML personnalisé

Les étapes suivantes décrivent comment créer et déployer un hook SAML personnalisé.

### Créer l’implémentation du hook SAML

Créez une nouvelle classe Java dans le projet AEM qui implémente l&#39;interface `com.adobe.granite.auth.saml.spi.SamlHook` :

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### Configuration du hook SAML

Le hook SAML utilise la configuration OSGi pour spécifier à quel FI il doit s’appliquer. Créez un fichier de configuration OSGi dans le projet à l’emplacement suivant :

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

`idpIdentifier` doit correspondre à la valeur `idpIdentifier` configurée dans la configuration de fabrique OSGi du gestionnaire d&#39;authentification SAML correspondant (PID : `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`). Cette correspondance est critique : le hook SAML sera appelé uniquement pour l&#39;instance du gestionnaire d&#39;authentification SAML qui a la même valeur `idpIdentifier`. Le gestionnaire d&#39;authentification SAML est une configuration d&#39;usine, ce qui signifie que vous pouvez avoir plusieurs instances (par exemple, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`) et chaque hook est lié à un gestionnaire spécifique via `idpIdentifier`. La propriété `service.ranking` contrôle l&#39;ordre d&#39;exécution lorsque plusieurs hooks sont configurés (les valeurs supérieures s&#39;exécutent en premier).

### Ajouter des dépendances Maven

Ajoutez la dépendance SPI SAML requise au `pom.xml` du projet principal AEM Maven.

**Pour les projets AEM as a Cloud Service**, utilisez la dépendance API SDK AEM qui inclut les interfaces SAML :

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

L’artefact `aem-sdk-api` contient toutes les interfaces SAML Granite Adobe nécessaires, y compris `com.adobe.granite.auth.saml.spi.SamlHook`.

### Configurer l’utilisateur du service (facultatif)

Si le hook SAML doit modifier le contenu du référentiel AEM JCR, tel que les propriétés de l&#39;utilisateur (comme indiqué dans l&#39;exemple `postSyncUserProcess`), un [utilisateur de service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) doit être configuré :

1. Créer un mappage d&#39;utilisateurs de service dans le projet à `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json` :

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Créez un script repoinit pour définir l&#39;utilisateur du service et les autorisations à `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json` :

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

Cela accorde aux utilisateurs du service les autorisations de lire et de modifier les propriétés utilisateur dans le référentiel.

### Déploiement sur AEM

Déployez le hook SAML personnalisé vers AEM as a Cloud Service :

1. Création du projet AEM
1. Valider le code dans le référentiel Git Cloud Manager
1. Déploiement à l’aide d’un pipeline de déploiement Full Stack
1. Le hook SAML est automatiquement activé lorsqu’un utilisateur s’authentifie via SAML


### Considérations importantes

+ **Correspondance de l&#39;identifiant du FI** : le `idpIdentifier` configuré dans le hook SAML doit correspondre exactement au `idpIdentifier` dans la configuration d&#39;usine du gestionnaire d&#39;authentification SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)
+ **Noms d’attributs** : vérifiez que les noms d’attributs référencés dans le hook (par exemple, `groupMembership`) correspondent aux attributs configurés dans le gestionnaire d’authentification SAML
+ **Performances** : réduisez la taille des implémentations de hook lors de leur exécution lors de chaque authentification SAML
+ **Gestion des erreurs** : les implémentations de hook SAML doivent lever `com.adobe.granite.auth.saml.spi.SamlHookException` lorsque des erreurs critiques se produisent et que l&#39;authentification doit échouer. Le gestionnaire d&#39;authentification SAML interceptera ces exceptions et renverra `AuthenticationInfo.FAIL_AUTH`. Pour les opérations de référentiel, interceptez toujours `RepositoryException` et consignez les erreurs de manière appropriée. Utiliser des blocs try-catch-finally pour assurer un nettoyage approprié des ressources
+ **Tests** : testez minutieusement les hooks personnalisés dans les environnements inférieurs avant de les déployer en production
+ **Hooks multiples** : plusieurs implémentations de hook SAML peuvent être configurées ; tous les hooks correspondants sont exécutés. Utilisez la propriété `service.ranking` dans le composant OSGi pour contrôler l’ordre d’exécution (les valeurs de rang supérieur s’exécutent en premier). Pour réutiliser un hook SAML sur plusieurs configurations d’usine du gestionnaire d’authentification SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`), créez plusieurs configurations de hook (configurations d’usine OSGi), chacune avec un `idpIdentifier` différent correspondant au gestionnaire d’authentification SAML correspondant
+ **Sécurité** : validez et assainissez toutes les données des assertions SAML avant de les utiliser dans la logique commerciale.
+ **Accès au référentiel** : lors de la modification des propriétés utilisateur dans `postSyncUserProcess`, utilisez toujours un [utilisateur de service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) avec les autorisations appropriées plutôt que des sessions administratives
+ **Autorisations utilisateur de service** : accordez les autorisations minimales requises à l&#39;[utilisateur de service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (par exemple, uniquement `jcr:read` et `rep:write` sur `/home/users`, pas les droits d&#39;administrateur complets)
+ **Gestion des sessions** : utilisez toujours des blocs try-catch-finally pour vous assurer que les sessions du référentiel sont correctement fermées, même en cas d&#39;exception
+ **Synchronisation de l&#39;utilisateur** : le hook `postSyncUserProcess` s&#39;exécute après la synchronisation de l&#39;utilisateur sur OAK. L&#39;objet utilisateur existe donc assurément dans le référentiel à ce stade
