---
title: SAML 2.0 sur AEM as a Cloud Service
description: Découvrez comment configurer l’authentification SAML 2.0 sur AEM service de publication as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: 343040.jpeg
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
source-git-commit: 5522a22cc3ac12ce54297ee9f30570c29cfd5ce7
workflow-type: tm+mt
source-wordcount: '2961'
ht-degree: 2%

---

# Authentification SAML 2.0{#saml-2-0-authentication}

Découvrez comment configurer et authentifier les utilisateurs finaux (et non AEM les auteurs) sur un fournisseur d’identité compatible SAML 2.0 de votre choix.

## Quel SAML pour AEM as a Cloud Service ?

L’intégration de SAML 2.0 à AEM Publish (ou Preview) permet aux utilisateurs finaux d’une expérience web basée sur AEM de s’authentifier auprès d’un fournisseur d’identité (fournisseur d’identité) non-Adobe et d’accéder à AEM en tant qu’utilisateur autorisé et nommé.

|  | Auteur AEM | Publication AEM |
|-----------------------|:----------:|:-----------:|
| Prise en charge de SAML 2.0 | ✘ | ✔ |

+++ Comprendre le flux SAML 2.0 avec AEM

Le flux type d’une intégration SAML de publication AEM est le suivant :

1. L’utilisateur envoie une requête à AEM Publish indiquant que l’authentification est requise.
   + L’utilisateur demande une ressource protégée CUG/ACL.
   + L’utilisateur demande une ressource soumise à une exigence d’authentification.
   + L’utilisateur suit un lien pour AEM point de terminaison de connexion (c.-à-d. `/system/sling/login`) qui demande explicitement l’action de connexion.
1. AEM envoie une requête d’auteur au fournisseur d’identité, demandant au fournisseur d’identité de démarrer le processus d’authentification.
1. L’utilisateur s’authentifie auprès du fournisseur d’identité.
   + L’utilisateur est invité par le fournisseur d’identité pour les informations d’identification.
   + L’utilisateur est déjà authentifié auprès du fournisseur d’identité et n’a pas à fournir d’informations d’identification supplémentaires.
1. IDP génère une assertion SAML contenant les données de l’utilisateur et la signe à l’aide du certificat privé du fournisseur d’identité.
1. IDP envoie l’assertion SAML via le POST HTTP, par le biais du navigateur web de l’utilisateur, à AEM Publish.
1. AEM Publish reçoit l’assertion SAML et valide l’intégrité et l’authenticité de l’assertion SAML à l’aide du certificat public IDP.
1. AEM Publish gère l’enregistrement utilisateur AEM en fonction de la configuration OSGi SAML 2.0 et du contenu de l’assertion SAML.
   + Crée un utilisateur
   + Synchronise les attributs utilisateur
   + Mises à jour AEM l’appartenance à un groupe d’utilisateurs
1. AEM Publish définit l’AEM `login-token` sur la réponse HTTP, qui est utilisée pour authentifier les requêtes suivantes vers AEM Publish.
1. AEM Publish redirige l’utilisateur vers l’URL sur AEM Publish, comme indiqué par le `saml_request_path` du cookie.

+++

## Présentation de la configuration

>[!VIDEO](https://video.tv.adobe.com/v/343040/?quality=12&learn=on)

Cette vidéo décrit la configuration de l’intégration SAML 2.0 avec AEM service de publication as a Cloud Service et l’utilisation d’Okta comme fournisseur d’identité.

## Prérequis

Les éléments suivants sont requis lors de la configuration de l’authentification SAML 2.0 :

+ Accès de Deployment Manager à Cloud Manager
+ Accès AEM administrateur à AEM environnement as a Cloud Service
+ Accès de l’administrateur au fournisseur d’identité
+ Éventuellement, l’accès à une paire de clés publique/privée utilisée pour le chiffrement des payloads SAML.

SAML 2.0 est uniquement pris en charge pour authentifier les utilisateurs dans AEM Publish ou Preview. Pour gérer l’authentification de l’auteur AEM à l’aide de et de IDP, [Intégration du fournisseur d’identité avec Adobe IMS](https://helpx.adobe.com/fr/enterprise/using/set-up-identity.html).


## Installer le certificat public IDP sur AEM

Le certificat public du fournisseur d’identité est ajouté à AEM Trust Store global et utilisé pour valider l’assertion SAML envoyée par le fournisseur d’identité est valide.

+++Flux de signature d’assertion SAML

![SAML 2.0 - Signature de l’assertion SAML IDP](./assets/saml-2-0/idp-signing-diagram.png)

1. L’utilisateur s’authentifie auprès du fournisseur d’identité.
1. IDP génère une assertion SAML contenant les données de l’utilisateur.
1. IDP signe l’assertion SAML à l’aide du certificat privé du fournisseur d’identité.
1. IDP lance un POST HTTP côté client au point de terminaison SAML de la publication AEM (`.../saml_login`) qui inclut l’assertion SAML signée.
1. AEM Publish reçoit le POST HTTP contenant l’assertion SAML signée, peut valider la signature à l’aide du certificat public IDP.

+++

![Ajout du certificat public IDP au Trust Store global](./assets/saml-2-0/global-trust-store.png)

1. Obtenez la variable __certificat public__ à partir du IDP. Ce certificat permet à AEM de valider l’assertion SAML fournie à AEM par le fournisseur d’identité.

   Le certificat est au format PEM et doit ressembler à :

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Connectez-vous à AEM Author en tant qu’administrateur AEM.
1. Accédez à __Outils > Sécurité > Trust Store__.
1. Créez ou ouvrez le Trust Store global. Si vous créez un Trust Store global, stockez le mot de passe en lieu sûr.
1. Développer __Ajout d’un certificat à partir du fichier CER__.
1. Sélectionner __Sélectionner le fichier de certificat__ et chargez le fichier de certificat fourni par le fournisseur d’identité.
1. Laissez tomber __Mappage du certificat à l’utilisateur__ vide.
1. Sélectionnez __Envoyer__.
1. Le certificat nouvellement ajouté s’affiche au-dessus du __Ajout d’un certificat à partir d’un fichier CRT__ .
1. Prenez note de la __alias__, car cette valeur est utilisée dans la variable [Configuration OSGi du gestionnaire d’authentification SAML 2.0](#saml-2-0-authentication-handler-osgi-configuration).
1. Sélectionner __Enregistrer et fermer__.

Le Trust Store global est configuré avec le certificat public du fournisseur d’identité sur l’auteur AEM, mais comme SAML est utilisé uniquement sur AEM Publish, le Trust Store global doit être répliqué vers AEM Publish pour que le certificat public IDP y soit accessible.

![Réplication du Trust Store global vers AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. Accédez à __Outils > Déploiement > Packages__.
1. Créer un package
   + Nom du module : `Global Trust Store`
   + d’Adobe Experience Manager Forms 6.5: `1.0.0`
   + Groupe : `com.your.company`
1. Modifiez la nouvelle __Trust Store mondial__ module.
1. Sélectionnez la __Filtres__ et ajouter un filtre pour le chemin d’accès racine. `/etc/truststore`.
1. Sélectionner __Terminé__ puis __Enregistrer__.
1. Sélectionnez la __Build__ pour le bouton __Trust Store mondial__ module.
1. Une fois la création terminée, sélectionnez __Plus__ > __Répliquer__ pour activer le noeud Trust Store global (`/etc/truststore`) vers AEM Publish.

## Création d’un KeyStore de service d’authentification{#authentication-service-keystore}

_La création d’un KeyStore pour authentication-service est requise lorsque la variable [Propriété de configuration OSGi du gestionnaire d’authentification SAML 2.0 `handleLogout` est défini sur `true`](#saml-20-authenticationsaml-2-0-authentication) ou [AuthnRequest signing/SAML assertion ecryption](#install-aem-public-private-key-pair) est requis_

1. Connectez-vous à l’auteur AEM en tant qu’administrateur AEM pour charger la clé privée.
1. Accédez à __Outils > Sécurité > Trust Store__, puis sélectionnez __authentication-service__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Accédez à __Outils > Sécurité > Utilisateurs__, puis sélectionnez __authentication-service__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Sélectionnez la __Keystore__ .
1. Créez ou ouvrez le KeyStore. Si vous créez un fichier de stockage de clés, assurez-vous que le mot de passe est sécurisé.
   + A [le KeyStore public/privé est installé dans ce KeyStore.](#install-aem-public-private-key-pair) uniquement si le chiffrement de la signature AuthnRequest/de l’assertion SAML est requis.
   + Si cette intégration SAML prend en charge la déconnexion, mais pas l’assertion de signature/SAML AuthnRequest, alors un fichier de stockage de clés vide est suffisant.
1. Sélectionner __Enregistrer et fermer__.
1. Sélectionner __authentication-service__ et sélectionnez __Activer__ dans la barre d’actions supérieure.


## Installer AEM paire de clés publique/privée{#install-aem-public-private-key-pair}

_L’installation de la paire de clés publique/privée AEM est facultative._

La publication AEM peut être configurée pour signer les demandes d’auteur (IDP) et chiffrer les assertions SAML (pour AEM). Pour ce faire, vous devez fournir une clé privée à AEM Publish et faire correspondre la clé publique au fournisseur d’identité.

+++ Comprendre le flux de signature AuthnRequest (facultatif)

La demande d’auteur (la demande au fournisseur d’identité d’AEM Publish qui initie le processus de connexion) peut être signée par AEM Publish. Pour ce faire, AEM Publish signe la requête d’auteur à l’aide de la clé privée, que le fournisseur d’identité valide ensuite la signature à l’aide de la clé publique. Cela garantit au fournisseur d’identité que la demande d’auteur a été lancée et demandée par AEM Publish, et non un tiers malveillant.

![SAML 2.0 - Signature de la requête d’auteur SP](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. L’utilisateur envoie une requête HTTP à AEM Publish qui entraîne une requête d’authentification SAML au fournisseur d’identité.
1. AEM Publish génère la requête SAML à envoyer au fournisseur d’identité.
1. AEM Publish signe la requête SAML à l’aide d’AEM clé privée.
1. AEM Publish lance la requête d’auteur, une redirection HTTP côté client vers le fournisseur d’identité qui contient la requête SAML signée.
1. IDP reçoit la demande d’auteur et valide la signature à l’aide de AEM clé publique, ce qui garantit que AEM Publish a lancé la demande d’auteur.
1. AEM Publish valide ensuite l’intégrité et l’authenticité de l’assertion SAML déchiffrée à l’aide du certificat public IDP.

+++

+++ Comprendre le flux de chiffrement de l’assertion SAML (facultatif)

Toutes les communications HTTP entre IDP et AEM Publish doivent se faire via HTTPS, et donc être sécurisées par défaut. Cependant, si nécessaire, les assertions SAML peuvent être chiffrées dans l’événement, une confidentialité supplémentaire est requise en plus de celle fournie par HTTPS. Pour ce faire, le fournisseur d’identité chiffre les données d’assertion SAML à l’aide de la clé privée et AEM Publish déchiffre l’assertion SAML à l’aide de la clé privée.

![SAML 2.0 - Chiffrement de l’assertion SAML SP](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. L’utilisateur s’authentifie auprès du fournisseur d’identité.
1. IDP génère une assertion SAML contenant les données de l’utilisateur et la signe à l’aide du certificat privé du fournisseur d’identité.
1. IDP chiffre ensuite l’assertion SAML avec AEM clé publique, ce qui nécessite la clé privée AEM pour le déchiffrer.
1. L’assertion SAML chiffrée est envoyée, par le biais du navigateur web de l’utilisateur, à AEM Publish.
1. AEM Publish reçoit l’assertion SAML et la décrypte à l’aide d’AEM clé privée.
1. IDP invite l’utilisateur à s’authentifier.

+++

La signature de la requête d’auteur et le chiffrement de l’assertion SAML sont facultatifs. Toutefois, ils sont tous deux activés, à l’aide de la propriété [Propriété de configuration OSGi du gestionnaire d’authentification SAML 2.0 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication), ce qui signifie qu’il est possible d’utiliser les deux ou aucun des deux.

![AEM entrepôt de clés authentication-service](./assets/saml-2-0/authentication-service-key-store.png)

1. Procurez-vous la clé publique, la clé privée (PKCS#8 au format DER) et le fichier de chaîne de certificats (il peut s’agir de la clé publique) utilisés pour signer la demande d’auteur et chiffrer l’assertion SAML. Les clés sont généralement fournies par l’équipe de sécurité de l’entreprise informatique.

   + Une paire de clés auto-signée peut être générée à l’aide de __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Téléchargez la clé publique vers le fournisseur d’identité.
   + En utilisant la variable `openssl` , la clé publique est la méthode `aem-public.crt` fichier .
1. Connectez-vous à l’auteur AEM en tant qu’administrateur AEM pour charger la clé privée.
1. Accédez à __Outils > Sécurité > Trust Store__, puis sélectionnez __authentication-service__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Accédez à __Outils > Sécurité > Utilisateurs__, puis sélectionnez __authentication-service__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Sélectionnez la __Keystore__ .
1. Créez ou ouvrez le KeyStore. Si vous créez un fichier de stockage de clés, assurez-vous que le mot de passe est sécurisé.
1. Sélectionner __Ajout d’une clé privée à partir du fichier DER__, puis ajoutez la clé privée et le fichier de chaîne à AEM :
   + __Alias__: Attribuez un nom significatif, souvent le nom du fournisseur d’identité.
   + __Fichier de clé privée__: Téléchargez le fichier de clé privée (PKCS#8 au format DER).
      + En utilisant la variable `openssl` , il s’agit de la méthode `aem-private-pkcs8.der` fichier
   + __Sélectionner le fichier de chaîne de certificat__: Téléchargez le fichier de chaîne d’accompagnement (il peut s’agir de la clé publique).
      + En utilisant la variable `openssl` , il s’agit de la méthode `aem-public.crt` fichier
   + Sélectionnez __Envoyer__
1. Le certificat nouvellement ajouté s’affiche au-dessus du __Ajout d’un certificat à partir d’un fichier CRT__ .
   + Prenez note de la __alias__ car il est utilisé dans la variable [Configuration OSGi du gestionnaire d’authentification SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Sélectionner __Enregistrer et fermer__.
1. Sélectionner __authentication-service__ et sélectionnez __Activer__ dans la barre d’actions supérieure.

## Configuration du gestionnaire d’authentification SAML 2.0{#configure-saml-2-0-authentication-handler}

AEM configuration SAML est effectuée via la fonction __Gestionnaire d’authentification SAML 2.0 Adobe__ Configuration OSGi.
La configuration est une configuration d’usine OSGi, ce qui signifie qu’un service de publication as a Cloud Service unique peut avoir plusieurs configurations SAML couvrant des arborescences de ressources discrètes du référentiel ; cela s’avère utile pour les déploiements d’AEM multi-site.

+++ Glossaire de configuration OSGi du gestionnaire d’authentification SAML 2.0

### Configuration OSGi du gestionnaire d’authentification SAML 2.0 Adobe{#configure-saml-2-0-authentication-handler-osgi-configuration}

|  | Propriété OSGi | Requise | Format de valeur | Valeur par défaut | Description |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Chemins d’accès | `path` | ✔ | Tableau de chaînes | `/` | AEM chemins d’accès pour lesquels ce gestionnaire d’authentification est utilisé. |
| URL IDP | `idpUrl` | ✔ | Chaîne |  | URL IDP La demande d’authentification SAML est envoyée. |
| ID certificate | `idpCertAlias` | ✔ | Chaîne |  | L’alias du certificat IDP trouvé dans AEM Trust Store global |
| Redirection HTTP IDP | `idpHttpRedirect` | ✘ | Booléen | `false` | Indique si une redirection HTTP vers l’URL IDP au lieu d’envoyer une requête d’auteur. Définissez sur . `true` pour l’authentification initiée par IDP. |
| Identifiant IDP | `idpIdentifier` | ✘ | Chaîne |  | Identifiant IDP unique pour garantir AEM unicité des utilisateurs et des groupes. Si elle est vide, la variable `serviceProviderEntityId` est utilisée à la place. |
| URL du service client d’assertion | `assertionConsumerServiceURL` | ✘ | Chaîne |  | Le `AssertionConsumerServiceURL` attribut d’URL dans la requête d’auteur spécifiant où `<Response>` doit être envoyé à AEM. |
| Identifiant d’entité SP | `serviceProviderEntityId` | ✔ | Chaîne |  | Identifie de manière unique l’AEM au fournisseur d’identité ; généralement le nom d’hôte AEM. |
| Cryptage SP | `useEncryption` | ✘ | Booléen | `true` | Indique si le fournisseur d’identité chiffre les assertions SAML. Nécessite `spPrivateKeyAlias` et `keyStorePassword` à définir. |
| Alias de clé privée SP | `spPrivateKeyAlias` | ✘ | Chaîne |  | L’alias de la clé privée dans la variable `authentication-service` boutique de clés de l’utilisateur. Obligatoire si `useEncryption` est défini sur `true`. |
| mot de passe du stockage de clés SP | `keyStorePassword` | ✘ | Chaîne |  | Mot de passe du stockage des clés de l’utilisateur &#39;authentication-service&#39;. Obligatoire si `useEncryption` est défini sur `true`. |
| Redirection par défaut | `defaultRedirectUrl` | ✘ | Chaîne | `/` | URL de redirection par défaut après une authentification réussie. Peut être relatif à l’hôte AEM (par exemple, `/content/wknd/us/en/html`). |
| Attribut d’identifiant utilisateur | `userIDAttribute` | ✘ | Chaîne | `uid` | Nom de l’attribut d’assertion SAML contenant l’identifiant utilisateur de l’utilisateur AEM. Laissez vide pour utiliser la variable `Subject:NameId`. |
| Création automatique d’utilisateurs AEM | `createUser` | ✘ | Booléen | `true` | Indique si AEM utilisateurs ont été créés lors d’une authentification réussie. |
| AEM chemin intermédiaire de l’utilisateur | `userIntermediatePath` | ✘ | Chaîne |  | Lors de la création d’utilisateurs AEM, cette valeur est utilisée comme chemin intermédiaire (par exemple, `/home/users/<userIntermediatePath>/jane@wknd.com`). Nécessite `createUser` à définir sur `true`. |
| Attributs d’utilisateur AEM | `synchronizeAttributes` | ✘ | Tableau de chaînes |  | Liste des mappages d’attributs SAML à stocker sur l’utilisateur AEM, au format `[ "saml-attribute-name=path/relative/to/user/node" ]` (par exemple, `[ "firstName=profile/givenName" ]`). Voir [liste complète des attributs d’AEM natifs](#aem-user-attributes). |
| Ajout d’un utilisateur aux groupes AEM | `addGroupMemberships` | ✘ | Booléen | `true` | Indique si un utilisateur AEM est automatiquement ajouté aux groupes d’utilisateurs AEM après une authentification réussie. |
| Attribut AEM appartenance au groupe | `groupMembershipAttribute` | ✘ | Chaîne | `groupMembership` | Le nom de l’attribut d’assertion SAML contenant une liste des groupes d’utilisateurs AEM auxquels l’utilisateur doit être ajouté. Nécessite `addGroupMemberships` à définir sur `true`. |
| Groupes d’AEM par défaut | `defaultGroups` | ✘ | Tableau de chaînes |  | Une liste de groupes d’utilisateurs AEM auxquels des utilisateurs authentifiés sont toujours ajoutés (par exemple : `[ "wknd-user" ]`). Nécessite `addGroupMemberships` à définir sur `true`. |
| NomIDPolicy Format | `nameIdFormat` | ✘ | Chaîne | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | La valeur du paramètre de format NameIDPolicy à envoyer dans le message AuthnRequest. |
| Réponse SAML du magasin | `storeSAMLResponse` | ✘ | Booléen | `false` | Indique si la variable `samlResponse` est stockée sur l’AEM `cq:User` noeud . |
| Gérer la déconnexion | `handleLogout` | ✘ | Booléen | `false` | Indique si la demande de déconnexion est gérée par ce gestionnaire d’authentification SAML. Nécessite `logoutUrl` à définir. |
| URL de déconnexion | `logoutUrl` | ✘ | Chaîne |  | URL du fournisseur d’identité à laquelle la demande de déconnexion SAML est envoyée. Obligatoire si `handleLogout` est défini sur `true`. |
| Tolérance de l&#39;horloge | `clockTolerance` | ✘ | Entier | `60` | IDP et AEM (SP) réduisent la tolérance lors de la validation des assertions SAML. |
| méthode Digest | `digestMethod` | ✘ | Chaîne | `http://www.w3.org/2001/04/xmlenc#sha256` | Algorithme de résumé utilisé par le fournisseur d’identité lors de la signature d’un message SAML. |
| Méthode de signature | `signatureMethod` | ✘ | Chaîne | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Algorithme de signature utilisé par le fournisseur d’identité lors de la signature d’un message SAML. |
| Type de synchronisation des identités | `identitySyncType` | ✘ | `default` ou `idp` | `default` | Ne pas modifier `from` par défaut pour AEM as a Cloud Service. |
| Classement des services | `service.ranking` | ✘ | Entier | `5002` | Les configurations de rang supérieur sont préférées pour le même `path`. |

### Attributs d’utilisateur AEM{#aem-user-attributes}

AEM utilise les attributs utilisateur suivants, qui peuvent être renseignés via la variable `synchronizeAttributes` dans la configuration OSGi du gestionnaire d’authentification SAML 2.0 Adobe Granite.  Tous les attributs IDP peuvent être synchronisés avec n’importe quelle propriété d’utilisateur AEM, mais le mappage à AEM les propriétés d’attribut d’utilisation (répertoriées ci-dessous) permet à l’utilisateur d’y être naturellement associé.

| Attribut utilisateur | Chemin de propriété relatif depuis `rep:User` node |
|--------------------------------|--------------------------|
| Titre (par exemple, `Mrs`) | `profile/title` |
| Prénom (prénom) | `profile/givenName` |
| Nom de famille (nom) | `profile/familyName` |
| Titre de la tâche | `profile/jobTitle` |
| Adresse e-mail | `profile/email` |
| Adresse | `profile/street` |
| Ville | `profile/city` |
| Code postal | `profile/postalCode` |
| Pays | `profile/country` |
| Numéro de téléphone | `profile/phoneNumber` |
| A propos de moi | `profile/aboutMe` |

+++

1. Créez un fichier de configuration OSGi dans votre projet à l’adresse `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` et ouvrez dans votre IDE.
   + Modifier `/wknd-examples/` à `/<project name>/`
   + L’identifiant après la variable `~` dans le nom de fichier doit identifier de manière unique cette configuration. Il peut donc s’agir du nom du fournisseur d’identité, tel que `...~okta.cfg.json`. La valeur doit être alphanumérique avec des tirets.
1. Collez le code JSON suivant dans le `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` et mettez à jour le fichier `wknd` références selon les besoins.

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

1. Mettez à jour les valeurs comme requis par votre projet. Voir __Glossaire de configuration OSGi du gestionnaire d’authentification SAML 2.0__ ci-dessus pour les descriptions des propriétés de configuration
1. Il est recommandé, mais non obligatoire, d’utiliser des variables et des secrets d’environnement OSGi lorsque les valeurs peuvent ne pas être synchronisées avec le cycle de publication ou lorsque les valeurs diffèrent entre les types d’environnements/niveaux de service similaires. Les valeurs par défaut peuvent être définies à l’aide de la variable `$[env:..;default=the-default-value]"` comme illustré ci-dessus.

Configurations OSGi par environnement (`config.publish.dev`, `config.publish.stage`, et `config.publish.prod`) peut être défini avec des attributs spécifiques si la configuration SAML varie d’un environnement à l’autre.

### Utiliser le chiffrement

When [cryptage de l’assertion AuthnRequest et SAML](#encrypting-the-authnrequest-and-saml-assertion), les propriétés suivantes sont requises : `useEncryption`, `spPrivateKeyAlias`, et `keyStorePassword`. Le `keyStorePassword` contient un mot de passe. Par conséquent, la valeur ne doit pas être stockée dans le fichier de configuration OSGi, mais plutôt injectée à l’aide de [valeurs de configuration secrètes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++Si vous le souhaitez, mettez à jour la configuration OSGi pour utiliser le chiffrement.

1. Ouvrir `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` dans votre IDE.
1. Ajouter les trois propriétés `useEncryption`, `spPrivateKeyAlias`, et `keyStorePassword` comme illustré ci-dessous.

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

1. Les trois propriétés de configuration OSGi requises pour le chiffrement sont les suivantes :

+ `useEncryption` Défini sur `true`
+ `spPrivateKeyAlias` contient l’alias d’entrée du KeyStore pour la clé privée utilisée par l’intégration SAML.
+ `keyStorePassword` contient un [Variable de configuration secrète OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) contenant le `authentication-service` mot de passe du fichier de stockage des clés utilisateur.

+++

## Configuration du filtre de référent

Pendant le processus d’authentification SAML, le fournisseur d’identité lance un POST HTTP côté client vers le serveur de publication AEM `.../saml_login` point de terminaison. Si le fournisseur d’identité et la publication AEM existent sur une origine différente, le __Filtre de référent__ est configuré via la configuration OSGi pour autoriser les POST HTTP à partir de l’origine du fournisseur d’identité.

1. Créez (ou modifiez) un fichier de configuration OSGi dans votre projet à l’adresse `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Modifier `/wknd-examples/` à `/<project name>/`
1. Assurez-vous que la variable `allow.empty` est définie sur `true`, la variable `allow.hosts` (ou si vous préférez, `allow.hosts.regexp`) contient l’origine du fournisseur d’identité, et `filter.methods` inclut `POST`. La configuration OSGi doit être similaire à :

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

AEM Publish prend en charge une configuration de filtre de référent unique. Par conséquent, fusionnez les exigences de configuration SAML avec toutes les configurations existantes.

Configurations OSGi par environnement (`config.publish.dev`, `config.publish.stage`, et `config.publish.prod`) peut être défini avec des attributs spécifiques si la variable `allow.hosts` (ou `allow.hosts.regex`) varient selon les environnements.

## Configurez le partage des ressources cross-origin (CORS)

Pendant le processus d’authentification SAML, le fournisseur d’identité lance un POST HTTP côté client vers le serveur de publication AEM `.../saml_login` point de terminaison. Si le fournisseur d’identité et la publication AEM existent sur différents hôtes/domaines, le __Partage des ressources CRoss-Origin (CORS)__ doit être configuré pour autoriser les instructions POST HTTP à partir de l’hôte/du domaine du fournisseur d’identité.

Cette requête de POST HTTP `Origin` L’en-tête a généralement une valeur différente de celle de l’hôte de publication AEM, ce qui nécessite une configuration CORS.

Lors du test de l’authentification SAML sur le SDK AEM local (`localhost:4503`), le fournisseur d’identité peut définir la variable `Origin` en-tête à `null`. Si tel est le cas, ajoutez `"null"` au `alloworigin` liste.

1. Créez un fichier de configuration OSGi dans votre projet à l’adresse `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Modifier `/wknd-examples/` dans le nom du projet
   + L’identifiant après la variable `~` dans le nom de fichier doit identifier de manière unique cette configuration. Il peut donc s’agir du nom du fournisseur d’identité, tel que `...CORSPolicyImpl~okta.cfg.json`. La valeur doit être alphanumérique avec des tirets.
1. Collez le code JSON suivant dans le `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` fichier .

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

Configurations OSGi par environnement (`config.publish.dev`, `config.publish.stage`, et `config.publish.prod`) peut être défini avec des attributs spécifiques si la variable `alloworigin` et `allowedpaths` varie d’un environnement à l’autre.

## Configuration d’AEM Dispatcher pour autoriser les POST HTTP SAML

Une fois l’authentification au fournisseur d’identité réussie, le fournisseur d’identité orchestrera un POST HTTP vers AEM enregistré. `/saml_login` point de terminaison (configuré dans le fournisseur d’identité). Ce POST HTTP vers `/saml_login` est bloqué par défaut sur Dispatcher. Il doit donc être autorisé explicitement à l’aide de la règle Dispatcher suivante :

1. Ouvrir `dispatcher/src/conf.dispatcher.d/filters/filters.any` dans votre IDE.
1. Ajoutez au bas du fichier une règle permettant d’autoriser les publications HTTP aux URL se terminant par `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

Si la réécriture de l’URL sur le serveur web Apache est configurée (`dispatcher/src/conf.d/rewrites/rewrite.rules`), assurez-vous que les requêtes de la variable `.../saml_login` les points de fin ne sont pas gérés accidentellement.

## Activation de la synchronisation des données et encapsulation des jetons

Une fois que le flux d’authentification SAML crée un utilisateur dans AEM Publish, le noeud utilisateur AEM authentifiable au niveau du service de publication AEM.
Cela nécessite [synchronisation des données](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#data-synchronization) et [jetons encapsulés](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#sticky-sessions-and-encapsulated-tokens) pour être activé par l’assistance Adobe sur le service de publication AEM.

Envoyez une demande au service clientèle d’Adobe (via [Admin Console](https://adminconsole.adobe.com) > Assistance) demandant :

> La synchronisation des données et les jetons encapsulés sont activés sur le service AEM Publish pour le programme X et l’environnement Y.

## Déploiement de la configuration SAML

Les configurations OSGi doivent être validées dans Git et déployées vers AEM as a Cloud Service à l’aide de Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Déployer la branche Git Cloud Manager cible (dans cet exemple, `develop`), à l’aide d’un pipeline de déploiement Stack complet.
