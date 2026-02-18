---
title: SAML 2.0 sur AEM as a Cloud Service
description: Découvrez comment configurer l’authentification SAML 2.0 sur le service de publication AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 4a8d97d8d65f0ff9b256cb233db5dd6a70fd2a8a
workflow-type: tm+mt
source-wordcount: '5215'
ht-degree: 80%

---

# Authentification SAML 2.0{#saml-2-0-authentication}

Découvrez comment configurer et authentifier les utilisateurs finaux (et non les auteurs et autrices AEM) sur le fournisseur d’identité compatible avec SAML 2.0 de votre choix.

## Quel SAML pour AEM as a Cloud Service ?

L’intégration de SAML 2.0 au service de publication (ou de prévisualisation) AEM permet aux utilisateurs et utilisatrices finaux d’une expérience web basée sur AEM de s’authentifier auprès d’un fournisseur d’identité hors Adobe et d’accéder à AEM en tant qu’utilisateur autorisé et nommé.

|                       | Création AEM | Publication AEM |
|-----------------------|:----------:|:-----------:|
| Prise en charge de SAML 2.0 | ✘ | ✔ |

+++ Comprendre le flux SAML 2.0 avec AEM

Le flux type d’une intégration SAML de publication AEM est le suivant :

1. L’utilisateur ou l’utilisatrice envoie une requête au service de publication AEM indiquant que l’authentification est requise.
   + L’utilisateur ou utilisatrice demande une ressource protégée par CUG/ACL.
   + L’utilisateur ou utilisatrice demande une ressource soumise à une exigence d’authentification.
   + L’utilisateur ou utilisatrice suit un lien vers le point d’entrée de connexion AEM (c.-à-d. `/system/sling/login`) qui demande explicitement l’action de connexion.
1. AEM envoie une requête AuthnRequest au fournisseur d’identité, demandant au fournisseur d’identité de démarrer le processus d’authentification.
1. L’utilisateur ou utilisatrice s’authentifie auprès du fournisseur d’identité.
   + Le fournisseur d’identité demande à l’utilisateur ou l’utilisatrice ses informations d’identification.
   + L’utilisateur ou l’utilisatrice est déjà authentifié(e) auprès du fournisseur d’identité et n’a pas à fournir d’informations d’identification supplémentaires.
1. Le fournisseur d’identité génère une assertion SAML contenant les données de l’utilisateur ou de l’utilisatrice et la signe à l’aide du certificat privé du fournisseur d’identité.
1. Le fournisseur d’identité envoie l’assertion SAML via une requête HTTP POST, par le biais du navigateur web de l’utilisateur ou de l’utilisatrice (RESPECTIVE_PROTECTED_PATH/saml_login), au service de publication AEM.
1. Le service de publication AEM reçoit l’assertion SAML et valide l’intégrité et l’authenticité de l’assertion SAML à l’aide du certificat public du fournisseur d’identité.
1. Le service de publication AEM gère l’enregistrement de l’utilisateur ou de l’utilisatrice AEM en fonction de la configuration OSGi SAML 2.0 et du contenu de l’assertion SAML.
   + Crée un utilisateur ou une utilisatrice
   + Synchronise les attributs utilisateur
   + Met à jour l’appartenance à un groupe d’utilisateurs ou d’utilisatrices AEM
1. Le service de publication AEM définit le cookie `login-token` AEM sur la réponse HTTP, qui est utilisée pour authentifier les requêtes suivantes vers le service de publication AEM.
1. Le service de publication AEM redirige l’utilisateur ou utilisatrice vers l’URL sur l’instance de publication AEM, comme indiqué par le cookie `saml_request_path`.

+++

## Présentation de la configuration

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Cette vidéo décrit comment configurer l’intégration SAML 2.0 au service de publication AEM as a Cloud Service et utiliser Okta comme fournisseur d’identité.

## Prérequis

Les éléments suivants sont requis lors de la configuration de l’authentification SAML 2.0 :

+ Un accès responsable de déploiement à Cloud Manager.
+ Un accès administratif AEM à l’environnement AEM as a Cloud Service.
+ Accès de l’administrateur ou de l’administratrice au fournisseur d’identité
+ Éventuellement, l’accès à une paire de clés publique/privée utilisée pour le chiffrement des payloads SAML.
+ Pages AEM Sites (ou arborescences de pages), publiées sur l’instance de publication AEM et [protégées par des groupes d’utilisateurs fermés (CUG)](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0 est uniquement pris en charge pour authentifier les utilisateurs et utilisatrices dans le service de publication ou de prévisualisation AEM. Pour gérer l’authentification du service de création AEM à l’aide d’un fournisseur d’identité, [intégrez le fournisseur d’identité à Adobe IMS](https://helpx.adobe.com/fr/enterprise/using/set-up-identity.html).


## Installer le certificat public du fournisseur d’identité sur AEM

Le certificat public du fournisseur d’identité est ajouté à AEM Global Trust Store et utilisé pour vérifier que l’assertion SAML envoyée par le fournisseur d’identité est valide.

+++Flux de signature d’assertion SAML

![SAML 2.0 - Signature de l’assertion SAML du fournisseur d’identité](./assets/saml-2-0/idp-signing-diagram.png)

1. L’utilisateur ou utilisatrice s’authentifie auprès du fournisseur d’identité.
1. Ce dernier génère une assertion SAML contenant les données de la personne.
1. Le fournisseur d’identité signe l’assertion SAML à l’aide du certificat privé du fournisseur d’identité.
1. Le fournisseur d’identité effectue une requête POST HTTP côté client au point d’entrée SAML de l’instance de publication AEM (`.../saml_login`), qui inclut l’assertion SAML signée.
1. L’instance de publication AEM reçoit la requête POST HTTP contenant l’assertion SAML signée et peut valider la signature à l’aide du certificat public du fournisseur d’identité.

+++

![Ajout du certificat public du fournisseur d’identité au Trust Store global](./assets/saml-2-0/global-trust-store.png).

1. Récupérez le fichier de __certificat public__ à partir du fournisseur d’identité. Ce certificat permet à AEM de valider l’assertion SAML transmise à AEM par le fournisseur d’identité.

   Le certificat, au format PEM, ressemble à ceci :

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Connectez-vous à l’instance de création AEM en tant qu’administrateur ou administratrice AEM.
1. Accédez à __Outils > Sécurité > Trust Store__.
1. Créez ou ouvrez le Trust Store global. Si vous créez un Trust Store global, stockez le mot de passe en lieu sûr.
1. Développez __Ajouter le certificat à partir du fichier CER__.
1. Sélectionnez __Sélectionner le fichier de certificat__ et chargez le fichier de certificat fourni par le fournisseur d’identité.
1. Laisser le champ __Mapper le certificat à l’utilisateur__ vide.
1. Sélectionnez __Envoyer__.
1. Le certificat nouvellement ajouté s’affiche au-dessus de la section __Ajouter un certificat à partir du fichier CRT__.
1. Prenez note de la valeur de l’__alias__, car celle-ci est utilisée dans la [Configuration OSGi du gestionnaire d’authentification SAML 2.0](#saml-2-0-authentication-handler-osgi-configuration).
1. Sélectionnez __Enregistrer et fermer__.

Le Trust Store global est configuré avec le certificat public du fournisseur d’identité sur l’instance de création AEM, mais comme SAML est utilisé uniquement sur l’instance de publication AEM, le Trust Store global doit être répliqué dans l’instance de publication AEM pour que le certificat public du fournisseur d’identité y soit accessible.

![Réplication du Trust Store global dans l’instance de publication AEM](./assets/saml-2-0/global-trust-store-replicate.png).

1. Accédez à __Outils > Déploiement > Packages__.
1. Créez un package.
   + Nom du package : `Global Trust Store`
   + Version : `1.0.0`
   + Groupe : `com.your.company`
1. Modifiez le nouveau package __Trust Store global__.
1. Sélectionnez l’onglet __Filtres__ et ajoutez un filtre pour le chemin d’accès racine `/etc/truststore`.
1. Sélectionnez __Terminé__, puis __Enregistrer__.
1. Cliquez sur le bouton __Créer__ pour le package __Trust Store global__.
1. Une fois créé, sélectionnez __Plus__ > __Répliquer__ pour activer le nœud Trust Store global (`/etc/truststore`) vers l’instance de publication AEM.

## Créer un KeyStore pour le service d’authentification{#authentication-service-keystore}

_La création d’un KeyStore pour le service d’authentification est requise lorsque la [Propriété de configuration OSGi du gestionnaire d’authentification SAML 2.0 `handleLogout` est définie sur `true`](#saml-20-authenticationsaml-2-0-authentication) ou lorsque la [signature de la demande d’autorisation/le chiffrement de l’assertion SAML](#install-aem-public-private-key-pair) est requis(e)_.

1. Connectez-vous à l’instance de création AEM en tant qu’administrateur ou administratrice AEM pour charger la clé privée.
1. Accédez à __Outils > Sécurité > Utilisateurs et utilisatrices__, puis sélectionnez __authentication-service__ et choisissez __Propriétés__ dans la barre d’actions supérieure.
1. Sélectionnez l’onglet __KeyStore__.
1. Créez ou ouvrez le KeyStore. Si vous créez un KeyStore, assurez-vous de conserver le mot de passe en lieu sûr.
   + Un [KeyStore public/privé est installé dans ce KeyStore](#install-aem-public-private-key-pair) uniquement si la signature de la demande d’authentification/le chiffrement de l’assertion SAML est requis(e).
   + Si cette intégration SAML prend en charge la déconnexion, mais pas la signature de la demande d’authentification/l’assertion SAML, alors un KeyStore vide est suffisant.
1. Sélectionnez __Enregistrer et fermer__.
1. Créez un package contenant l’utilisateur ou l’utilisatrice de l’__authentication-service__ mis à jour.

   _Utilisez la solution temporaire suivante à l&#39;aide de packages :_

   1. Accédez à __Outils > Déploiement > Packages__.
   1. Créez un package.
      + Nom du package : `Authentication Service`
      + Version : `1.0.0`
      + Groupe : `com.your.company`
   1. Modifiez le nouveau package __KeyStore du service d’authentification__.
   1. Sélectionnez l’onglet __Filtres__ et ajoutez un filtre pour le chemin racine `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + L’`<AUTHENTICATION SERVICE UUID>` est accessible en accédant à __Outils > Sécurité > Utilisateurs et utilisatrices__ et en sélectionnant l’utilisateur ou l’utilisatrice d’__authentication-service__. L’UUID est la dernière partie de l’URL.
   1. Sélectionnez __Terminé__, puis __Enregistrer__.
   1. Sélectionnez le bouton __Créer__ pour le package __KeyStore du service d’authentification__.
   1. Une fois créé, sélectionnez __Plus__ > __Répliquer__ pour activer le KeyStore du service d’authentification dans le service de publication AEM.

## Installer la paire de clés publique/privée AEM{#install-aem-public-private-key-pair}

_L’installation de la paire de clés publique/privée AEM est facultative._

Le service de publication AEM peut être configuré pour signer les requêtes AuthnRequests (pour le fournisseur d’identité) et chiffrer les assertions SAML (pour AEM). Pour ce faire, vous devez fournir une clé privée au service de publication AEM et faire correspondre la clé publique au fournisseur d’identité.

+++ Comprendre le flux de signature AuthnRequest (facultatif)

La requête AuthnRequest (la requête au fournisseur d’identité du service de publication AEM qui initie le processus de connexion) peut être signée par le service de publication AEM. Pour ce faire, le service de publication AEM signe la requête AuthnRequest à l’aide de la clé privée, puis le fournisseur d’identité valide la signature à l’aide de la clé publique. Cela garantit au fournisseur d’identité que la requête AuthnRequest a été lancée et demandée par le service de publication AEM, et non un tiers malveillant.

![SAML 2.0 - Signature de la requête AuthnRequest SP.](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. L’utilisateur ou l’utilisatrice envoie une requête HTTP au service de publication AEM qui entraîne une requête d’authentification SAML au fournisseur d’identité.
1. Le service de publication AEM génère la requête SAML à envoyer au fournisseur d’identité.
1. Le service de publication AEM signe la requête SAML à l’aide de la clé privée AEM.
1. Le service de publication AEM lance la requête AuthnRequest, une redirection HTTP côté client vers le fournisseur d’identité qui contient la requête SAML signée.
1. Le fournisseur d’identité reçoit la requête AuthnRequest et valide la signature à l’aide de la clé publique AEM, ce qui garantit que le servie de publication AEM a lancé la requête AuthnRequest.
1. Le service de publication AEM valide ensuite l’intégrité et l’authenticité de l’assertion SAML déchiffrée à l’aide du certificat public du fournisseur d’identité.

+++

+++ Comprendre le flux de chiffrement de l’assertion SAML (facultatif)

Toutes les communications HTTP entre le fournisseur d’identité et le service de publication AEM doivent se faire via HTTPS, et sont donc sécurisées par défaut. Cependant, si nécessaire, les assertions SAML peuvent être chiffrées si une confidentialité supplémentaire est requise en plus de celle fournie par HTTPS. Pour ce faire, le fournisseur d’identité chiffre les données d’assertion SAML à l’aide de la clé privée et le service de publication AEM déchiffre l’assertion SAML à l’aide de la clé privée.

![SAML 2.0 - Chiffrement de l’assertion SAML SP.](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. L’utilisateur ou utilisatrice s’authentifie auprès du fournisseur d’identité.
1. Le fournisseur d’identité génère une assertion SAML contenant les données de l’utilisateur ou de l’utilisatrice et la signe à l’aide du certificat privé du fournisseur d’identité.
1. Le fournisseur d’identité chiffre ensuite l’assertion SAML avec la clé publique AEM, qui nécessite la clé privée AEM pour être déchiffrée.
1. L’assertion SAML chiffrée est envoyée par le biais du navigateur web de l’utilisateur ou de l’utilisatrice au service de publication AEM.
1. Le service de publication AEM reçoit l’assertion SAML et la déchiffre à l’aide de la clé privée AEM.
1. Le fournisseur d’identité invite l’utilisateur ou l’utilisatrice à s’authentifier.

+++

La signature de la requête AuthnRequest et le chiffrement de l’assertion SAML sont facultatifs. Toutefois, ils sont tous deux activés à l’aide de la [propriété de configuration OSGi du gestionnaire d’authentification SAML 2.0`useEncryption`](#saml-20-authenticationsaml-2-0-authentication), ce qui signifie qu’il est possible d’utiliser les deux ou aucun des deux.

![KeyStore du service d’authentification AEM.](./assets/saml-2-0/authentication-service-key-store.png)

1. Procurez-vous la clé publique, la clé privée (PKCS#8 au format DER) et le fichier de chaîne de certificats (il peut s’agir de la clé publique) utilisés pour signer la requête AuthnRequest et chiffrer l’assertion SAML. Les clés sont généralement fournies par l’équipe de sécurité du service informatique.

   + Une paire de clés auto-signée peut être générée à l’aide de __openssl__ :

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Chargez la clé publique vers le fournisseur d’identité.
   + En utilisant la méthode `openssl` ci-dessus, la clé publique est le fichier `aem-public.crt`.
1. Connectez-vous à l’instance de création AEM en tant qu’administrateur ou administratrice AEM pour charger la clé privée.
1. Accédez à __Outils > Sécurité > Trust Store__, puis sélectionnez la personne qui utilise __authentication-service__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Accédez à __Outils > Sécurité > Utilisateurs et utilisatrices__, puis sélectionnez la personne qui utilise __authentication-service__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Sélectionnez l’onglet __KeyStore__.
1. Créez ou ouvrez le KeyStore. Si vous créez un KeyStore, assurez-vous de conserver le mot de passe en lieu sûr.
1. Sélectionner __Ajouter une clé privée à partir du fichier DER__, puis ajoutez la clé privée et le fichier de chaîne à AEM :
   + __Alias__ : indiquez un nom significatif, souvent le nom du fournisseur d’identité.
   + __Fichier de clé privée__ : chargez le fichier de clé privée (PKCS#8 au format DER).
      + En utilisant la méthode `openssl` ci-dessus, il s’agit du fichier `aem-private-pkcs8.der`.
   + __Sélectionner le fichier de chaîne de certificats__ : chargez le fichier de chaîne associé (il peut s’agir de la clé publique).
      + Avec la méthode `openssl` ci-dessus, il s’agit du fichier `aem-public.crt`.
   + Sélectionnez __Envoyer__.
1. Le certificat nouvellement ajouté s’affiche au-dessus de la section __Ajouter un certificat à partir d’un fichier CRT__.
   + Prenez note de l’__alias__, car il est utilisé dans la [Configuration OSGi du gestionnaire d’authentification SAML 2.0](#saml-20-authentication-handler-osgi-configuration).
1. Sélectionnez __Enregistrer et fermer__.
1. Créez un package contenant l’utilisateur ou l’utilisatrice de l’__authentication-service__ mis à jour.

   _Utilisez la solution temporaire suivante à l&#39;aide de packages :_

   1. Accédez à __Outils > Déploiement > Packages__.
   1. Créez un package.
      + Nom du package : `Authentication Service`
      + Version : `1.0.0`
      + Groupe : `com.your.company`
   1. Modifiez le nouveau package __KeyStore du service d’authentification__.
   1. Sélectionnez l’onglet __Filtres__ et ajoutez un filtre pour le chemin racine `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + L’`<AUTHENTICATION SERVICE UUID>` est accessible en accédant à __Outils > Sécurité > Utilisateurs et utilisatrices__ et en sélectionnant l’utilisateur ou l’utilisatrice d’__authentication-service__. L’UUID est la dernière partie de l’URL.
   1. Sélectionnez __Terminé__, puis __Enregistrer__.
   1. Sélectionnez le bouton __Créer__ pour le package __KeyStore du service d’authentification__.
   1. Une fois créé, sélectionnez __Plus__ > __Répliquer__ pour activer le KeyStore du service d’authentification dans l’instance de publication AEM.

## Configurer le gestionnaire d’authentification SAML 2.0.{#configure-saml-2-0-authentication-handler}

La configuration SAML d’AEM est effectuée via la configuration OSGi du __gestionnaire d’authentification SAML 2.0 Adobe Granite__.
La configuration est une configuration d’usine OSGi, ce qui signifie qu’un service de publication AEM as a Cloud Service unique peut avoir plusieurs configurations SAML couvrant des arborescences de ressources discrètes du référentiel ; cela est utile pour les déploiements AEM de plusieurs sites.

+++ Glossaire de configuration OSGi du gestionnaire d’authentification SAML 2.0.

### Configuration OSGi du gestionnaire d’authentification SAML 2.0 Adobe Granite{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | Propriété OSGi | Requis | Format de la valeur | Valeur par défaut | Description |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Chemins d’accès | `path` | ✔ | Tableau de chaînes | `/` | Chemins d’accès AEM pour lesquels ce gestionnaire d’authentification est utilisé. |
| URL du fournisseur d’identité | `idpUrl` | ✔ | Chaîne |                           | URL du fournisseur d’identité vers lequel la demande d’authentification SAML est envoyée. |
| Alias du certificat du fournisseur d’identité | `idpCertAlias` | ✔ | Chaîne |                           | Alias du certificat du fournisseur d’identité trouvé dans le Trust Store global d’AEM. |
| Redirection HTTP du fournisseur d’identité | `idpHttpRedirect` | ✘ | Booléen | `false` | Indique s’il existe une redirection du HTTP vers l’URL du fournisseur d’identité au lieu d’envoyer une demande d’authentification. Définissez `true` pour l’authentification initiée par fournisseur d’identité. |
| Identifiant du fournisseur d’identité | `idpIdentifier` | ✘ | Chaîne |                           | Identifiant du fournisseur d’identité unique pour garantir l’unicité des utilisateurs et utilisatrices ainsi que des groupes. Si le champ est vide, la valeur de `serviceProviderEntityId` est utilisée à la place. |
| URL du service client d’assertion | `assertionConsumerServiceURL` | ✘ | Chaîne |                           | Attribut d’URL `AssertionConsumerServiceURL` dans la requête d’authentification spécifiant où le message `<Response>` doit être envoyé à AEM. |
| Identifiant d’entité SP | `serviceProviderEntityId` | ✔ | Chaîne |                           | Identifie de manière unique AEM auprès du fournisseur d’identité. Il s’agit généralement du nom d’hôte d’AEM. |
| Chiffrement SP | `useEncryption` | ✘ | Booléen | `true` | Indique si le fournisseur d’identité chiffre les assertions SAML. Nécessite la définition des valeurs pour `spPrivateKeyAlias` et `keyStorePassword`. |
| Alias de clé privée SP | `spPrivateKeyAlias` | ✘ | Chaîne |                           | Alias de la clé privée dans le KeyStore de l’utilisateur ou de l’utilisatrice d’`authentication-service`. Obligatoire si `useEncryption` est défini sur `true`. |
| Mot de passe du KeyStore SP | `keyStorePassword` | ✘ | Chaîne |                           | Mot de passe du KeyStore de l’utilisateur ou de l’utilisatrice d’« authentication-service ». Obligatoire si `useEncryption` est défini sur `true`. |
| Redirection par défaut | `defaultRedirectUrl` | ✘ | Chaîne | `/` | URL de redirection par défaut après une authentification réussie. Il peut s’agir de l’hôte d’AEM (par exemple, `/content/wknd/us/en/html`). |
| Attribut d’identifiant d’utilisation | `userIDAttribute` | ✘ | Chaîne | `uid` | Nom de l’attribut d’assertion SAML contenant l’identifiant d’utilisation de l’utilisateur ou de l’utilisatrice AEM. Laissez vide pour utiliser l’`Subject:NameId`. |
| Créer automatiquement des utilisateurs et des utilisatrices AEM | `createUser` | ✘ | Booléen | `true` | Indique si les utilisateurs et utilisatrices AEM sont créés lorsque l’authentification aboutit. |
| Chemin intermédiaire de l’utilisateur ou de l’utilisatrice AEM | `userIntermediatePath` | ✘ | Chaîne |                           | Lors de la création d’utilisateurs et d’utilisatrices AEM, cette valeur est utilisée comme chemin intermédiaire (par exemple, `/home/users/<userIntermediatePath>/jane@wknd.com`). Nécessite que `createUser` soit défini sur `true`. |
| Attributs d’utilisateur ou d’utilisatrice AEM | `synchronizeAttributes` | ✘ | Tableau de chaînes |                           | Liste des mappages d’attributs SAML à stocker sur l’utilisateur ou l’utilisatrice AEM, au format `[ "saml-attribute-name=path/relative/to/user/node" ]` (par exemple, `[ "firstName=profile/givenName" ]`). Consultez la [liste complète des attributs AEM natifs](#aem-user-attributes). |
| Ajouter un utilisateur ou une utilisatrice aux groupes AEM | `addGroupMemberships` | ✘ | Booléen | `true` | Indique si une personne qui utilise AEM est automatiquement ajoutée aux groupes d’utilisateurs et d’utilisatrices AEM après une authentification réussie. |
| Attribut d’appartenance à un groupe AEM | `groupMembershipAttribute` | ✘ | Chaîne | `groupMembership` | Le nom de l’attribut d’assertion SAML contenant une liste des groupes d’utilisateurs et d’utilisatrices AEM auxquels la personne doit être ajoutée. Nécessite que `addGroupMemberships` soit défini sur `true`. |
| Groupes AEM par défaut | `defaultGroups` | ✘ | Tableau de chaînes |                           | Une liste de groupes d’utilisateurs et d’utilisatrices AEM auxquels des personnes authentifiées sont toujours ajoutées (par exemple : `[ "wknd-user" ]`). Nécessite que `addGroupMemberships` soit défini sur `true`. |
| Format NameIDPolicy | `nameIdFormat` | ✘ | Chaîne | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | La valeur du paramètre de format NameIDPolicy à envoyer dans le message AuthnRequest. |
| Stocker la réponse SAML | `storeSAMLResponse` | ✘ | Booléen | `false` | Indique si la valeur `samlResponse` est stockée sur le nœud `cq:User` d’AEM. |
| Gérer la déconnexion | `handleLogout` | ✘ | Booléen | `false` | Indique si la demande de déconnexion est gérée par ce gestionnaire d’authentification SAML. Nécessite que `logoutUrl` soit défini. |
| URL de déconnexion | `logoutUrl` | ✘ | Chaîne |                           | URL du fournisseur d’identité vers laquelle la requête de déconnexion SAML est envoyée. Obligatoire si `handleLogout` est défini sur `true`. |
| Tolérance de l’horloge | `clockTolerance` | ✘ | Entier | `60` | Tolérance de décalage de l’horloge du fournisseur d’identité et AEM (SP) lors de la validation des assertions SAML. |
| Méthode de prétraitement | `digestMethod` | ✘ | Chaîne | `http://www.w3.org/2001/04/xmlenc#sha256` | Algorithme de prétraitement (Digest) utilisé par le fournisseur d’identité lors de la signature d’un message SAML. |
| Méthode de signature | `signatureMethod` | ✘ | Chaîne | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Algorithme de signature utilisé par le fournisseur d’identité lors de la signature d’un message SAML. |
| Type de synchronisation des identités | `identitySyncType` | ✘ | `default` ou `idp` | `default` | Ne pas modifier la valeur par défaut `from` pour AEM as a Cloud Service. |
| Classement de service | `service.ranking` | ✘ | Entier | `5002` | Les configurations de classement supérieur sont préférées pour le même `path`. |

### Attributs d’utilisateur ou d’utilisatrice AEM{#aem-user-attributes}

AEM utilise les attributs d’utilisateur ou d’utilisatrice suivants, qui peuvent être renseignés via la propriété `synchronizeAttributes` dans la configuration OSGi du gestionnaire d’authentification SAML 2.0 Granite Adobe.  Tous les attributs de fournisseur d’identité peuvent être synchronisés avec n’importe quelle propriété d’utilisateur ou d’utilisatrice AEM. Toutefois, le mappage aux propriétés d’attribut d’utilisation AEM (répertoriées ci-dessous) permet à AEM de les utiliser naturellement.

| Attribut d’utilisateur ou d’utilisatrice | Chemin de propriété relatif depuis le nœud `rep:User` |
|--------------------------------|--------------------------|
| Titre (par exemple, `Mrs`) | `profile/title` |
| Prénom | `profile/givenName` |
| Nom de famille | `profile/familyName` |
| Fonction | `profile/jobTitle` |
| Adresse e-mail | `profile/email` |
| Adresse | `profile/street` |
| Ville | `profile/city` |
| Code postal | `profile/postalCode` |
| Pays | `profile/country` |
| Numéro de téléphone | `profile/phoneNumber` |
| À propos de moi | `profile/aboutMe` |

+++

1. Créez un fichier de configuration OSGi dans votre projet à l’emplacement `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` et ouvrez-le dans votre IDE.
   + Remplacez `/wknd-examples/` par votre `/<project name>/`.
   + L’identifiant après le `~` dans le nom de fichier doit identifier de manière unique cette configuration. Il peut donc s’agir du nom du fournisseur d’identité, tel que `...~okta.cfg.json`. La valeur doit être alphanumérique avec des tirets.
1. Collez le code JSON suivant dans le fichier `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` et mettez à jour les références `wknd` selon les besoins.

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. Mettez à jour les valeurs en fonction de votre projet. Consultez le __glossaire de configuration OSGi du gestionnaire d’authentification SAML 2.0__ ci-dessus pour obtenir la description des propriétés de configuration. Le `path` doit contenir les arborescences de contenu protégées par des groupes d’utilisateurs fermés (CUG) et nécessitant une authentification, et ce gestionnaire d’authentification doit être chargé de la protection.
1. Il est recommandé, mais non obligatoire, d’utiliser les variables d’environnement et les secrets OSGi lorsque les valeurs peuvent ne pas être synchronisées avec le cycle de publication de la version ou lorsque les valeurs diffèrent entre des types d’environnements/niveaux de service similaires. Les valeurs par défaut peuvent être définies à l’aide de la syntaxe `$[env:..;default=the-default-value]"` comme illustré ci-dessus.

Les configurations OSGi par environnement (`config.publish.dev`, `config.publish.stage`, et `config.publish.prod`) peuvent être définies avec des attributs spécifiques si la configuration SAML varie entre les environnements.

### Utiliser le chiffrement

Lors du [chiffrement de l’assertion SAML et de la requête AuthnRequest](#encrypting-the-authnrequest-and-saml-assertion), les propriétés suivantes sont requises : `useEncryption`, `spPrivateKeyAlias`, et `keyStorePassword`. Le `keyStorePassword` contient un mot de passe. Par conséquent, la valeur ne doit pas être stockée dans le fichier de configuration OSGi, mais plutôt injectée à l’aide des [valeurs de configuration secrètes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=fr#secret-configuration-values).

+++Vous pouvez éventuellement mettre à jour la configuration OSGi pour utiliser le chiffrement

1. Ouvrez `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` dans votre IDE.
1. Ajoutez les trois propriétés `useEncryption`, `spPrivateKeyAlias`, et `keyStorePassword` comme illustré ci-dessous.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. Les trois propriétés de configuration OSGi requises pour le chiffrement sont les suivantes :

+ `useEncryption` défini sur `true`
+ `spPrivateKeyAlias` contient l’alias d’entrée du KeyStore pour la clé privée utilisée par l’intégration SAML.
+ `keyStorePassword` contient une [variable de configuration de secret OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=fr#secret-configuration-values) contenant le mot de passe du KeyStore de l’utilisateur ou de l’utilisatrice d’`authentication-service`.

+++

## Configurer le filtre de référent

Pendant le processus d’authentification SAML, le fournisseur d’identité lance une requête HTTP POST côté client vers le point d’entrée `.../saml_login` du service de publication AEM. Si le fournisseur d’identité et le service de publication AEM existent sur une origine différente, le __Filtre de référent__ du service de publication AEM est configuré via la configuration OSGi pour autoriser les requêtes HTTP POST à partir de l’origine du fournisseur d’identité.

1. Créez (ou modifiez) un fichier de configuration OSGi dans votre projet à l’emplacement `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Remplacez `/wknd-examples/` par votre `/<project name>/`.
1. Assurez-vous que la valeur `allow.empty` est définie sur `true`, `allow.hosts` (ou si vous préférez, `allow.hosts.regexp`) contient l’origine du fournisseur d’identité, et `filter.methods` inclut `POST`. La configuration OSGi doit être similaire à ce qui suit :

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

Le service de publication AEM prend en charge une configuration de filtre de référent unique. Par conséquent, fusionnez les exigences de configuration SAML avec toutes les configurations existantes.

Les configurations OSGi par environnement (`config.publish.dev`, `config.publish.stage`, et `config.publish.prod`) peuvent être définies avec des attributs spécifiques si `allow.hosts` (ou `allow.hosts.regex`) varie selon les environnements.

## Configurer le partage des ressources entre origines multiples (CORS)

Pendant le processus d’authentification SAML, le fournisseur d’identité lance une requête HTTP POST côté client vers le point d’entrée `.../saml_login` du service de publication AEM. Si le fournisseur d’identité et le service de publication AEM existent sur différents hôtes/domaines, le __partage de ressources entre origines multiples (CORS)__ du service de publication AEM doit être configuré pour autoriser les requêtes HTTP POST à partir de l’hôte/du domaine du fournisseur d’identité.

L’en-tête `Origin` de cette requête HTTP POST a généralement une valeur différente de celle de l’hôte du service de publication AEM, ce qui nécessite une configuration CORS.

Lors du test de l’authentification SAML sur le SDK AEM local (`localhost:4503`), le fournisseur d’identité peut définir l’en-tête `Origin` sur `null`. Si c’est le cas, ajoutez `"null"` à la liste `alloworigin`.

1. Créez un fichier de configuration OSGi dans votre projet à l’adresse `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`.
   + Modifiez `/wknd-examples/` en indiquant le nom du projet.
   + L’identifiant après `~` dans le nom de fichier doit identifier de manière unique cette configuration. Il peut donc s’agir du nom du fournisseur d’identité, tel que `...CORSPolicyImpl~okta.cfg.json`. La valeur doit être alphanumérique avec des tirets.
1. Collez le JSON suivant dans le fichier `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json`.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

Les configurations OSGi par environnement (`config.publish.dev`, `config.publish.stage` et `config.publish.prod`) peuvent être définies avec des attributs spécifiques si `alloworigin` et `allowedpaths` varient entre les environnements.

## Configurer le Dispatcher AEM pour autoriser les requêtes POST HTTP SAML

Une fois l’authentification au fournisseur d’identité réussie, le fournisseur d’identité orchestre une requête POST HTTP vers le point d’entrée `/saml_login` enregistré AEM (configuré dans le fournisseur d’identité). Cette requête POST HTTP vers `/saml_login` est bloqué par défaut sur Dispatcher. Elle doit donc être autorisée explicitement à l’aide de la règle Dispatcher suivante :

1. Ouvrez `dispatcher/src/conf.dispatcher.d/filters/filters.any` dans votre IDE.
1. Ajoutez au bas du fichier une règle permettant d’autoriser les publications HTTP aux URL se terminant par `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Lors du déploiement de plusieurs configurations SAML dans AEM pour divers chemins protégés et points d’entrée IDP distincts, assurez-vous que le fournisseur d’identité publie au point d’entrée RESPECTIVE_PROTECTED_PATH/saml_login pour sélectionner la configuration SAML appropriée côté AEM. S’il existe des configurations SAML en double pour le même chemin protégé, la sélection de la configuration SAML se produit de manière aléatoire.

Si la réécriture de l’URL sur le serveur web Apache est configurée (`dispatcher/src/conf.d/rewrites/rewrite.rules`), assurez-vous que les requêtes vers les points d’entrée `.../saml_login` ne sont pas accidentellement tronquées.

## Appartenance à un groupe dynamique

L’appartenance à un groupe dynamique est une fonctionnalité d’[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) qui améliore les performances de l’évaluation et de la mise en service des groupes. Cette section décrit comment les utilisateurs et utilisatrices et les groupes sont stockés lorsque cette fonctionnalité est activée et comment modifier la configuration du gestionnaire d’authentification SAML pour l’activer pour les environnements nouveaux ou existants.

### Activation de l’appartenance à un groupe dynamique pour les utilisateurs et utilisatrices SAML dans de nouveaux environnements

Pour améliorer considérablement les performances d’évaluation de groupe dans de nouveaux environnements AEM as a Cloud Service, l’activation de la fonctionnalité d’appartenance à un groupe dynamique est recommandée dans de nouveaux environnements.
Cette étape est également nécessaire lorsque la synchronisation des données est activée. Plus de détails [ici](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier).
Pour ce faire, ajoutez la propriété suivante au fichier de configuration OSGI :

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Avec cette configuration, les utilisateurs et utilisatrices et les groupes sont créés en tant qu’[&#x200B; utilisateurs et utilisatrices externes d’Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). Dans AEM, les utilisateurs et utilisatrices et les groupes externes ont une valeur par défaut `rep:principalName` composée des éléments `[user name];[idp]` ou `[group name];[idp]`.
Notez que les listes de contrôle d’accès (ACL) sont associées au PrincipalName des utilisateurs et utilisatrices ou des groupes.
Lors du déploiement de cette configuration dans un déploiement existant où `identitySyncType` n’a pas été auparavant spécifié ou défini sur `default`, de nouveaux utilisateurs, utilisatrices et groupes sont créés et des listes de contrôle d’accès doivent être appliquées à ces nouveaux utilisateurs, utilisatrices et groupes. Notez que les groupes externes ne peuvent pas contenir d’utilisateurs et d’utilisatrices locaux. [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html) peut être utilisé pour créer des listes de contrôle d’accès pour les groupes externes SAML, même s’ils ne sont créés que lorsque l’utilisateur ou l’utilisatrice effectue une connexion.
Pour éviter cette refactorisation sur les listes de contrôle d’accès, une [fonctionnalité de migration](#automatic-migration-to-dynamic-group-membership-for-existing-environments) standard a été mise en œuvre.

### Stockage des adhésions dans des groupes locaux et externes avec une appartenance à un groupe dynamique

Sur les groupes locaux, les personnes membres du groupe sont stockées dans l’attribut oak : `rep:members`. L’attribut contient la liste des uid de chaque personne membre du groupe. Vous trouverez des détails supplémentaires [ici](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
Exemple :

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

Les groupes externes dotés d’une appartenance à un groupe dynamique ne stockent aucune personne membre dans l’entrée de groupe.
L’appartenance à un groupe est à la place stockée dans les entrées des utilisateurs et utilisatrices. Vous trouverez une documentation supplémentaire [ici](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Par exemple, il s’agit du nœud OAK du groupe :

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

Il s’agit du nœud pour une personne membre de ce groupe :

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### Activer l’appartenance à un groupe dynamique pour les utilisateurs et utilisatrices SAML dans des environnements existants

Comme expliqué dans la section précédente, le format des utilisateurs et utilisatrices et des groupes externes est légèrement différent de celui utilisé pour les utilisateurs et utilisatrices et groupes locaux. Il est possible de définir une nouvelle liste de contrôle d’accès pour des groupes externes et de configurer de nouveaux utilisateurs et utilisatrices externes, ou d’utiliser l’outil de migration comme décrit ci-dessous.

#### Activer l’appartenance à un groupe dynamique pour les environnements existants comportant des utilisateurs et utilisatrices externes

Le gestionnaire d’authentification SAML crée des utilisateurs et utilisatrices externes lorsque la propriété suivante est spécifiée : `"identitySyncType": "idp"`. Dans ce cas, l’appartenance à un groupe dynamique peut être activée en modifiant cette propriété sur : `"identitySyncType": "idp_dynamic"`. Aucune migration n’est requise.

#### Migration automatique vers l’appartenance à un groupe dynamique pour les environnements existants avec des utilisateurs et utilisatrices locaux

Le gestionnaire d’authentification SAML crée des utilisateurs et utilisatrices locaux lorsque la propriété suivante est spécifiée : `"identitySyncType": "default"`. Il s’agit également de la valeur par défaut lorsque la propriété n’est pas spécifiée. Cette section décrit les étapes de la procédure de migration automatique.

Lorsque cette migration est activée, elle est réalisée lors de l’authentification de l’utilisateur ou de l’utilisatrice et comprend les étapes suivantes :
1. La personne locale est migrée vers un profil utilisateur externe tout en conservant le nom d’utilisation d’origine. Cela signifie que les utilisateurs et utilisatrices locaux migrés, agissant désormais en tant qu’utilisateurs et utilisatrices externes, conservent leur nom d’utilisation d’origine au lieu de suivre la syntaxe d’affectation de nom mentionnée dans la section précédente. Une propriété supplémentaire sera ajoutée, appelée : `rep:externalId` avec la valeur `[user name];[idp]`. Le `PrincipalName` de la personne n’est pas modifié.
2. Pour chaque groupe externe reçu dans l’assertion SAML, un groupe externe est créé. S’il existe un groupe local correspondant, le groupe externe est ajouté en tant que membre au groupe local.
3. La personne est maintenant membre du groupe externe.
4. La personne locale est alors retirée de tous les groupes locaux SAML dont elle faisait partie. Les groupes locaux SAML sont identifiés par la propriété OAK : `rep:managedByIdp`. Cette propriété est définie par le gestionnaire d’authentification SAML lorsque l’attribut `syncType` n’est pas spécifié ou défini sur `default`.

Par exemple, si, avant la migration, `user1` est une personne locale et membre du groupe local `group1`, les modifications suivantes se produisent après la migration :
`user1` devient un utilisateur ou une utilisatrice externe. L’attribut `rep:externalId` est ajouté à son profil.
`user1` devient membre du groupe externe : `group1;idp`.
`user1` n’est plus un membre direct du groupe local : `group1`.
`group1;idp` est membre du groupe local : `group1`.
`user1` est alors membre du groupe local : `group1` par héritage.

L’appartenance à un groupe pour les groupes externes est stockée dans le profil d’utilisation dans la propriété `rep:externalPrincipalNames`.

### Configuration de la migration automatique vers l’appartenance à un groupe dynamique

1. Activez la propriété `"identitySyncType": "idp_dynamic_simplified_id"` dans le fichier de configuration SAML OSGI : `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` :
2. Configurez le nouveau service OSGi avec le PID d’usine commençant par : `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Par exemple, un PID peut être : `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`. Définissez la propriété suivante :

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Pour migrer plusieurs configurations SAML, plusieurs configurations d’usine OSGi pour `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` doivent être créées, chacune spécifiant un `idpIdentifier` à migrer.

## Hooks SAML personnalisés pour les cas d’utilisation avancés

Si le fournisseur d’identité ne peut pas envoyer les données de profil utilisateur et l’appartenance du groupe d’utilisateurs à l’assertion SAML, ou si les données doivent être transformées avant la synchronisation vers AEM, des hooks SAML personnalisés peuvent être implémentés pour étendre le processus d’authentification SAML. Les hooks SAML permettent de personnaliser l’affectation d’appartenance à un groupe, de modifier les attributs de profil utilisateur et d’ajouter une logique commerciale personnalisée pendant le flux d’authentification.

>[!NOTE]
>Les hooks SAML personnalisés sont pris en charge sur **AEM as a Cloud Service** et **AEM LTS**. Cette fonctionnalité n’est pas disponible sur les anciennes versions d’AEM.

### Quand utiliser des hooks SAML personnalisés ?

Les hooks SAML personnalisés sont utiles lorsqu’il est nécessaire de :

+ Attribuez de manière dynamique l’appartenance à un groupe en fonction d’une logique commerciale personnalisée au-delà de ce qui est fourni dans les assertions SAML
+ Transformation ou enrichissement des données de profil utilisateur avant leur synchronisation avec AEM
+ Mapper des structures d’attributs SAML complexes aux propriétés d’utilisateur AEM
+ Implémenter des règles d’autorisation personnalisées ou des affectations de groupe conditionnelles
+ Ajout d’une journalisation ou d’un audit personnalisé lors de l’authentification SAML
+ Intégration de systèmes externes pendant le processus d’authentification

### Présentation de l’interface SamlHook

L’interface `com.adobe.granite.auth.saml.spi.SamlHook` fournit deux méthodes de hook appelées à différentes étapes du processus d’authentification SAML :

#### 1. postSamlValidationProcess

Cette méthode est appelée **après** la validation de la réponse SAML, mais **avant** le lancement du processus de synchronisation des utilisateurs. Il s’agit de l’emplacement idéal pour modifier les données d’assertion SAML, telles que l’ajout ou la transformation d’attributs.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

**Cas d’utilisation :**
+ Ajoutez des appartenances de groupe supplémentaires à l’assertion
+ Transformation des valeurs d’attribut avant leur synchronisation
+ Enrichir l&#39;assertion avec des données provenant de sources externes
+ Validation des règles métier personnalisées

#### 2. postSyncUserProcess

Cette méthode est appelée **une fois** le processus de synchronisation des utilisateurs terminé. Ce hook peut être utilisé pour effectuer des opérations supplémentaires après la création ou la mise à jour de l’utilisateur AEM.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

**Cas d’utilisation :**
+ Mettre à jour des propriétés de profil utilisateur supplémentaires non couvertes par la synchronisation standard
+ Créer ou mettre à jour des ressources personnalisées liées aux utilisateurs dans AEM
+ Déclencher des workflows ou des notifications après l’authentification de l’utilisateur
+ Consigner les événements d’authentification personnalisés

**Important :** pour modifier les propriétés de l’utilisateur dans le référentiel, l’implémentation du hook requiert les éléments suivants :
+ Une référence `SlingRepository` injectée via `@Reference`
+ Un [utilisateur de service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) configuré avec les autorisations appropriées (configuré dans « Modification du service de mappage des utilisateurs du service Apache Sling »)
+ Gestion appropriée des sessions avec des blocs try-catch-finish

### Mise en œuvre d’un hook SAML personnalisé

Les étapes suivantes décrivent comment créer et déployer un hook SAML personnalisé :

#### Étape 1 : créer l’implémentation du hook SAML

Créez une classe Java dans le projet AEM qui implémente l’interface `com.adobe.granite.auth.saml.spi.SamlHook` :

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

#### Étape 2 : configurer le hook SAML

Le hook SAML utilise la configuration OSGi pour spécifier à quel fournisseur d’identité il doit s’appliquer. Créez un fichier de configuration OSGi dans le projet sous :

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

Le `idpIdentifier` doit correspondre à la valeur `idpIdentifier` configurée dans la configuration d’usine OSGi du gestionnaire d’authentification SAML correspondant (PID : `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`). Cette correspondance est essentielle : le hook SAML ne sera appelé que pour l’instance du gestionnaire d’authentification SAML qui a la même valeur `idpIdentifier`. Le gestionnaire d’authentification SAML est une configuration d’usine, ce qui signifie que vous pouvez avoir plusieurs instances (par exemple, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`) et chaque hook est lié à un gestionnaire spécifique via le `idpIdentifier`. La propriété `service.ranking` contrôle l’ordre d’exécution lorsque plusieurs hooks sont configurés (les valeurs les plus élevées sont exécutées en premier).

#### Étape 3 : ajouter des dépendances Maven

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

#### Étape 4 : configurer l’utilisateur du service (si vous modifiez le référentiel)

Si le hook SAML doit modifier les propriétés de l’utilisateur dans le référentiel (comme illustré dans l’exemple `postSyncUserProcess`), un [utilisateur du service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) doit être configuré :

1. Créez un mappage utilisateur de service dans le projet à l’adresse `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json` :

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Créez un script repoinit pour définir l’utilisateur du service et les autorisations au `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json` :

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

Cela permet à l’utilisateur du service de lire et de modifier les propriétés de l’utilisateur dans le référentiel.

#### Étape 5 : déploiement sur AEM

Déployez le hook SAML personnalisé vers AEM as a Cloud Service :

1. Créer le projet AEM
1. Validez le code dans le référentiel Git de Cloud Manager.
1. Déployer à l’aide d’un pipeline de déploiement Full Stack
1. Le hook SAML est automatiquement activé lorsqu’un utilisateur s’authentifie via SAML


### Considérations importantes

+ **Correspondance d’identifiants IDP** : le `idpIdentifier` configuré dans le hook SAML doit correspondre exactement au `idpIdentifier` dans la configuration d’usine du gestionnaire d’authentification SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)
+ **Noms d’attributs** : assurez-vous que les noms d’attributs référencés dans le hook (`groupMembership`, par exemple) correspondent aux attributs configurés dans le gestionnaire d’authentification SAML
+ **Performances** : allégez les implémentations de hook car elles sont exécutées lors de chaque authentification SAML
+ **Gestion des erreurs** : les implémentations de hook SAML doivent être déclenchées en cas d`com.adobe.granite.auth.saml.spi.SamlHookException`erreurs critiques qui doivent faire échouer l’authentification. Le gestionnaire d’authentification SAML interceptera ces exceptions et renverra des `AuthenticationInfo.FAIL_AUTH`. Pour les opérations de référentiel, capturez toujours les `RepositoryException` et consignez les erreurs de manière appropriée. Utilisez des blocs try-catch-finish pour garantir un nettoyage correct des ressources
+ **Tests** : testez minutieusement les hooks personnalisés dans les environnements inférieurs avant de les déployer en production
+ **Hooks multiples** : plusieurs implémentations de hook SAML peuvent être configurées ; tous les hooks correspondants sont exécutés. Utilisez la propriété `service.ranking` dans le composant OSGi pour contrôler l’ordre d’exécution (les valeurs de rang supérieur s’exécutent en premier). Pour réutiliser un hook SAML sur plusieurs configurations d’usine du gestionnaire d’authentification SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`), créez plusieurs configurations de hook (configurations d’usine OSGi), chacune avec un `idpIdentifier` différent correspondant au gestionnaire d’authentification SAML correspondant
+ **Sécurité** : validez et assainissez toutes les données des assertions SAML avant de les utiliser dans la logique commerciale.
+ **Accès au référentiel** : lors de la modification des propriétés utilisateur dans `postSyncUserProcess`, utilisez toujours un [utilisateur de service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) avec les autorisations appropriées plutôt que des sessions administratives
+ **Autorisations d’utilisateur du service** : accordez les autorisations minimales requises à l’[utilisateur du service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (par exemple, uniquement `jcr:read` et `rep:write` sur `/home/users`, et non les droits d’administrateur complets)
+ **Gestion des sessions** : utilisez toujours des blocs try-catch-finish pour vous assurer que les sessions du référentiel sont correctement fermées, même si des exceptions se produisent
+ **Synchronisation de l’utilisateur** : le hook `postSyncUserProcess` s’exécute après la synchronisation de l’utilisateur avec OAK. L’objet utilisateur existe donc bien dans le référentiel à ce stade

## Déployer la configuration SAML

Les configurations OSGi doivent être validées dans Git et déployées vers AEM as a Cloud Service à l’aide de Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Déployez la branche Git Cloud Manager cible (dans cet exemple, `develop`), à l’aide d’un pipeline de déploiement full stack.

## Appel de l’authentification SAML

Le flux d’authentification SAML peut être appelé à partir d’une page web d’un site AEM, en créant un lien spécialement conçu ou un bouton. Les paramètres décrits ci-dessous peuvent être définis par programmation selon les besoins. Par exemple, un bouton de connexion peut définir le `saml_request_path`, où la personne est amenée à accéder à différentes pages d’AEM en fonction du contexte du bouton.

## Mise en cache sécurisée lors de l’utilisation de SAML

Sur l’instance de publication AEM, la plupart des pages sont généralement mises en cache. Toutefois, pour les chemins protégés par SAML, la mise en cache doit être soit désactivée soit activée de manière sécurisée à l’aide de la configuration auth_checker. Pour plus d’informations, reportez-vous aux détails fournis [ici](https://experienceleague.adobe.com/fr/docs/experience-manager-dispatcher/using/configuring/permissions-cache).

Gardez à l’esprit que si vous mettez en cache des chemins protégés sans activer auth_checker, vous pouvez rencontrer un comportement imprévisible.

### Requête GET

L’authentification SAML peut être appelée en créant une requête HTTP GET au format suivant :

`HTTP GET /system/sling/login`

et en fournissant les paramètres de requête :

| Nom du paramètre de requête | Valeur du paramètre de requête |
|----------------------|-----------------------|
| `resource` | Tout chemin d’accès JCR, ou sous-chemin, sur lequel le gestionnaire d’authentification SAML écoute, comme défini dans la propriété `path` de la [configuration OSGi du gestionnaire d’authentification SAML 2.0 d’Adobe Granite](#configure-saml-2-0-authentication-handler). |
| `saml_request_path` | Chemin d’URL vers lequel la personne doit être dirigée après une authentification SAML réussie. |

Par exemple, ce lien HTML déclenche le flux de connexion SAML et, en cas de succès, la personne doit être dirigée vers `/content/wknd/us/en/protected/page.html`. Ces paramètres de requête peuvent être définis par programmation selon les besoins.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## Requête POST

L’authentification SAML peut être appelée en créant une requête HTTP POST au format suivant :

`HTTP POST /system/sling/login`

et en fournissant les données du formulaire :

| Nom des données de formulaire | Valeur des données de formulaire |
|----------------------|-----------------------|
| `resource` | Tout chemin d’accès JCR, ou sous-chemin, sur lequel le gestionnaire d’authentification SAML écoute, comme défini dans la propriété `path` de la [configuration OSGi du gestionnaire d’authentification SAML 2.0 d’Adobe Granite](#configure-saml-2-0-authentication-handler). |
| `saml_request_path` | Chemin d’URL vers lequel la personne doit être dirigée après une authentification SAML réussie. |


Par exemple, ce bouton HTML utilisera un POST HTTP pour déclencher le flux de connexion SAML. Une fois l’opération terminée, la personne sera dirigée vers `/content/wknd/us/en/protected/page.html`. Ces paramètres de données de formulaire peuvent être définis par programmation selon les besoins.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Configuration de Dispatcher

Les méthodes HTTP GET et POST requièrent un accès client aux points d’entrée `/system/sling/login` d’AEM. Elles doivent donc être autorisées via AEM Dispatcher.

Autorisation des modèles d’URL nécessaires en fonction de l’utilisation de GET ou POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
