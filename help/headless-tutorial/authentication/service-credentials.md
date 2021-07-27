---
title: Informations d’identification du service
description: Les informations d’identification du service AEM sont utilisées pour faciliter les applications, systèmes et services externes permettant d’interagir par programmation avec les services AEM Author ou Publish via HTTP.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: les API ;
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Sans affichage, intégrations
role: Developer
level: Intermediate, Experienced
source-git-commit: b902ced3d7f7cf827d0a487bf741ff370f7c1f04
workflow-type: tm+mt
source-wordcount: '1863'
ht-degree: 0%

---


# Informations d’identification du service

Les intégrations à AEM en tant que Cloud Service doivent pouvoir s’authentifier en toute sécurité dans AEM. AEM Developer Console accorde l’accès aux informations d’identification du service, qui sont utilisées pour faciliter les applications, systèmes et services externes pour interagir par programmation avec les services AEM Author ou Publish sur HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Les informations d’identification du service peuvent apparaître de la même manière [Jetons d’accès au développement local](./local-development-access-token.md), mais elles sont différentes de plusieurs manières clés :

+ Les informations d’identification du service sont _et non_ des jetons d’accès. Il s’agit plutôt des informations d’identification utilisées pour _obtenir_ des jetons d’accès.
+ Les informations d’identification du service sont plus permanentes (expirent tous les 365 jours) et ne changent pas, sauf si elles sont révoquées, tandis que les jetons d’accès au développement local expirent tous les jours.
+ Les informations d’identification du service pour un environnement AEM en tant que Cloud Service sont mappées à un utilisateur de compte technique unique AEM, tandis que les jetons d’accès au développement local s’authentifient en tant qu’utilisateur ayant généré le jeton d’accès.

Les informations d’identification du service et les jetons d’accès qu’ils génèrent, ainsi que les jetons d’accès au développement local, doivent être gardés secrets, car les trois peuvent être utilisés pour accéder à leur AEM respective en tant qu’environnements Cloud Service.

## Générer les informations d’identification du service

La génération des informations d’identification du service est divisée en deux étapes :

1. Initialisation ponctuelle des informations d’identification du service par un administrateur d’organisation IMS Adobe
1. Téléchargement et utilisation des informations d’identification du service JSON

### Initialisation des informations d’identification du service

Contrairement aux jetons d’accès de développement locaux, les informations d’identification du service requièrent une _initialisation unique_ de l’administrateur IMS de votre organisation d’Adobe avant de pouvoir être téléchargées.

![Initialisation des informations d’identification du service](assets/service-credentials/initialize-service-credentials.png)

__Il s’agit d’une initialisation unique par AEM en tant qu’environnement de Cloud Service.__

1. Vérifiez que vous êtes connecté en tant que :
   + Votre administrateur de l&#39;organisation IMS Adobe
   + Membre du __profil de produit Cloud Manager - Développeur__ IMS
   + Membre de __AEM User__ ou __AEM Administrateurs__ Profil de produit IMS sur __Auteur AEM__
1. Connectez-vous à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Ouvrez le programme contenant l’environnement AEM as a Cloud Service pour intégrer la configuration des informations d’identification du service pour
1. Appuyez sur les points de suspension en regard de l’environnement dans la section __Environnements__, puis sélectionnez __Developer Console__
1. Appuyez sur l’onglet __Intégrations__ .
1. Appuyez sur le bouton __Obtenir les informations d’identification du service__ .
1. Les informations d’identification du service seront initialisées et affichées au format JSON.

![AEM Developer Console - Intégrations - Obtention des informations d’identification du service](./assets/service-credentials/developer-console.png)

Une fois l’AEM comme informations d’identification de service de l’environnement de Cloud Service initialisé, d’autres développeurs AEM de votre organisation IMS d’Adobe peuvent les télécharger.

### Téléchargement des informations d’identification du service

![Téléchargement des informations d’identification du service](assets/service-credentials/download-service-credentials.png)

Le téléchargement des informations d’identification du service suit les mêmes étapes que l’initialisation. Si l’initialisation n’a pas encore eu lieu, l’utilisateur se verra présenter une erreur en appuyant sur le bouton __Obtenir les informations d’identification du service__.

1. Vérifiez que vous êtes connecté en tant que :
   + Membre du __Cloud Manager - Profil produit IMS du développeur__ (qui accorde l’accès à AEM Developer Console)
      + Les environnements Sandbox AEM en tant qu’environnements de Cloud Service ne nécessitent pas cette adhésion __Cloud Manager - Developer__
   + Membre de __AEM User__ ou __AEM Administrateurs__ Profil de produit IMS sur __Auteur AEM__
1. Connectez-vous à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Ouvrez le programme contenant l’AEM en tant qu’environnement de Cloud Service à intégrer à
1. Appuyez sur les points de suspension en regard de l’environnement dans la section __Environnements__, puis sélectionnez __Developer Console__
1. Appuyez sur l’onglet __Intégrations__ .
1. Appuyez sur le bouton __Obtenir les informations d’identification du service__ .
1. Appuyez sur le bouton de téléchargement dans le coin supérieur gauche pour télécharger le fichier JSON contenant la valeur Informations d’identification du service, puis enregistrez le fichier à un emplacement sécurisé.
   + _Si les informations d’identification du service sont compromises, contactez immédiatement l’assistance Adobe pour qu’elles soient révoquées._

## Installation des informations d’identification du service

Les informations d’identification du service fournissent les détails nécessaires à la génération d’un jeton JWT, qui est échangé contre un jeton d’accès utilisé pour s’authentifier avec AEM en tant que Cloud Service. Les informations d’identification du service doivent être stockées dans un emplacement sécurisé accessible par les applications, systèmes ou services externes qui l’utilisent pour accéder à AEM. Le mode et l’emplacement de gestion des informations d’identification du service seront uniques par client.

Pour plus de simplicité, ce tutoriel transmet les informations d’identification du service dans via la ligne de commande. Toutefois, travaillez avec votre équipe de sécurité informatique pour comprendre comment stocker ces informations d’identification et y accéder conformément aux directives de sécurité de votre entreprise.

1. Copiez le fichier [JSON des informations d’identification du service ](#download-service-credentials) téléchargé dans un fichier nommé `service_token.json` à la racine du projet.
   + Mais rappelez-vous, ne validez jamais vos identifiants à Git !

## Utilisation des informations d’identification du service

Les informations d’identification du service, un objet JSON entièrement formé, ne sont pas identiques au JWT ni au jeton d’accès. Les informations d’identification du service (qui contiennent une clé privée) sont utilisées pour générer un jeton d’accès JWT qui est échangé avec les API IMS d’Adobe.

![Informations d’identification du service - Application externe](assets/service-credentials/service-credentials-external-application.png)

1. Téléchargez les informations d’identification du service depuis AEM Developer Console vers un emplacement sécurisé.
1. Une application externe doit interagir par programmation avec AEM en tant qu’environnements de Cloud Service.
1. L’application externe lit les informations d’identification du service à partir d’un emplacement sécurisé.
1. L’application externe utilise les informations d’identification du service pour créer un jeton JWT.
1. Le jeton JWT est envoyé à l’Adobe IMS pour échanger contre un jeton d’accès.
1. Adobe IMS renvoie un jeton d’accès qui peut être utilisé pour accéder à AEM en tant que Cloud Service
   + Un délai d’expiration peut être demandé pour les jetons d’accès. Il est préférable de raccourcir la durée de vie du jeton d’accès et de l’actualiser si nécessaire.
1. L’application externe envoie des requêtes HTTP à AEM en tant que Cloud Service, en ajoutant le jeton d’accès en tant que jeton porteur à l’en-tête d’autorisation des requêtes HTTP.
1. AEM en tant que Cloud Service reçoit la requête HTTP, authentifie la requête et effectue le travail demandé par la requête HTTP, puis renvoie une réponse HTTP à l’application externe.

### Mises à jour de l’application externe

Pour accéder à AEM en tant que Cloud Service à l’aide des informations d’identification du service, votre application externe doit être mise à jour de trois façons :

1. Lecture dans les informations d’identification du service
   + Pour plus de simplicité, nous lirons ces informations à partir du fichier JSON téléchargé. Toutefois, dans les scénarios d’utilisation réelle, les informations d’identification du service doivent être stockées en toute sécurité conformément aux directives de sécurité de votre entreprise.
1. Génération d’un JWT à partir des informations d’identification du service
1. Échange du JWT pour un jeton d’accès
   + Lorsque les informations d’identification du service sont présentes, notre application externe utilise ce jeton d’accès au lieu du jeton d’accès au développement local lors de l’accès à AEM en tant que Cloud Service.

Dans ce tutoriel, le module npm `@adobe/jwt-auth` d’Adobe est utilisé pour les deux, (1) générer le JWT à partir des informations d’identification du service et (2) l’échanger contre un jeton d’accès, dans un seul appel de fonction. Si votre application n’est pas basée sur JavaScript, veuillez consulter l’[exemple de code dans d’autres langues](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) pour savoir comment créer un JWT à partir des informations d’identification du service et l’échanger contre un jeton d’accès avec l’Adobe IMS.

## Lire les informations d’identification du service

Passez en revue la balise `getCommandLineParams()` et vérifiez que nous pouvons lire les fichiers JSON des informations d’identification du service en utilisant le même code que celui utilisé pour lire le fichier JSON du jeton d’accès au développement local.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## Création d’un JWT et échange d’un jeton d’accès

Une fois les informations d’identification du service lues, elles sont utilisées pour générer un jeton d’accès JWT qui est ensuite échangé avec les API IMS d’Adobe pour un jeton d’accès, qui peut ensuite être utilisé pour accéder à AEM en tant que Cloud Service.

Cet exemple d’application est basé sur Node.js. Il est donc préférable d’utiliser le module npm [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) pour faciliter la génération (1) JWT et (20) l’échange avec Adobe IMS. Si votre application est développée à l’aide d’une autre langue, veuillez consulter [les exemples de code appropriés](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) sur la manière de construire la requête HTTP pour Adobe IMS à l’aide d’autres langages de programmation.

1. Mettez à jour `getAccessToken(..)` pour examiner le contenu du fichier JSON et déterminer s’il représente un jeton d’accès au développement local ou des informations d’identification du service. Pour ce faire, vérifiez l’existence de la propriété `.accessToken` , qui n’existe que pour le JSON JSON du jeton d’accès au développement local.

   Si les informations d’identification du service sont fournies, l’application génère un jeton JWT et l’échange avec un Adobe IMS pour un jeton d’accès. Nous utiliserons la fonction [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) de `auth(...)` qui génère toutes deux un JWT et l’échange pour un jeton d’accès dans un seul appel de fonction.  Les paramètres de `auth(..)` sont un objet [JSON constitué d’informations spécifiques](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponibles dans le fichier JSON Informations d’identification du service, comme décrit ci-dessous dans le code.

   ```javascript
    async function getAccessToken(developerConsoleCredentials) {
   
        if (developerConsoleCredentials.accessToken) {
            // This is a Local Development access token
            return developerConsoleCredentials.accessToken;
        } else {
            // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
            let serviceCredentials = developerConsoleCredentials.integration;
   
            // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
            // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
            let { access_token } = await auth({
                clientId: serviceCredentials.technicalAccount.clientId, // Client Id
                technicalAccountId: serviceCredentials.id,              // Technical Account Id
                orgId: serviceCredentials.org,                          // Adobe IMS Org Id
                clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
                privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
                metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
                ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
            });
   
            return access_token;
        }
    }
   ```

   Désormais, selon le fichier JSON transmis via ce paramètre de ligne de commande `file` (JSON du jeton d’accès au développement local ou JSON des informations d’identification du service), l’application obtient un jeton d’accès.

   N’oubliez pas que même si les informations d’identification du service expirent tous les 365 jours, le jeton d’accès JWT et le jeton d’accès correspondant expirent fréquemment et doivent être actualisés avant expiration. Pour ce faire, utilisez une balise `refresh_token` [fournie par l’Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Une fois ces modifications en place, et le fichier JSON Informations d’identification du service téléchargé à partir d’AEM Developer Console (et, pour plus de simplicité, enregistré sous la forme `service_token.json` du même dossier que ce `index.js`), exécutez l’application en remplaçant le paramètre de ligne de commande `file` par `service_token.json`, puis mettez à jour `propertyValue` vers une nouvelle valeur afin que les effets apparaissent dans AEM.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   La sortie vers le terminal se présentera comme suit :

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Les lignes __403 - Interdit__ indiquent des erreurs dans les appels API HTTP à AEM en tant que Cloud Service. Ces erreurs 403 interdites se produisent lors de la tentative de mise à jour des métadonnées des ressources.

   Cela s’explique par le fait que le jeton d’accès dérivé des informations d’identification du service authentifie la demande d’AEM à l’aide d’un compte technique créé automatiquement AEM utilisateur qui, par défaut, dispose uniquement d’un accès en lecture. Pour permettre l’accès en écriture de l’application à AEM, l’utilisateur du compte technique AEM associé au jeton d’accès doit se voir accorder l’autorisation dans l’.

## Configurer l’accès dans AEM

Le jeton d’accès dérivé des informations d’identification du service utilise un compte technique AEM utilisateur membre du groupe d’utilisateurs AEM contributeurs .

![Informations d’identification du service - Utilisateur AEM compte technique](./assets/service-credentials/technical-account-user.png)

Une fois que le compte technique AEM utilisateur existe dans AEM (après la première requête HTTP avec le jeton d’accès), les autorisations de cet utilisateur peuvent être gérées de la même manière que les autres utilisateurs .

1. Tout d’abord, recherchez le nom de connexion AEM du compte technique en ouvrant le fichier JSON des informations d’identification du service téléchargé depuis AEM Developer Console, puis recherchez la valeur `integration.email`, qui doit ressembler à : `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Connectez-vous au service Auteur de l’environnement AEM correspondant en tant qu’administrateur AEM
1. Accédez à __Outils__ > __Sécurité__ > __Utilisateurs__
1. Localisez l’utilisateur AEM avec le __nom de connexion__ identifié à l’étape 1, puis ouvrez ses __propriétés__
1. Accédez à l’onglet __Groupes__ et ajoutez le groupe __Utilisateurs de la gestion des actifs numériques__ (qui écrit les accès aux ressources).
1. Appuyez sur __Enregistrer et fermer__

Avec le compte technique autorisé dans AEM pour disposer d’autorisations d’écriture sur les ressources, exécutez de nouveau l’application :

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

La sortie vers le terminal se présentera comme suit :

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Vérifier les modifications

1. Connectez-vous à l’AEM en tant qu’environnement de Cloud Service mis à jour (en utilisant le même nom d’hôte que celui fourni dans le paramètre de ligne de commande `aem`).
1. Accédez à __Ressources__ > __Fichiers__
1. Accédez-y le dossier de ressources spécifié par le paramètre de ligne de commande `folder`, par exemple __WKND__ __Anglais__ > __Aventures__ > __Goûts du vin de Napa__
1. Ouvrez les __Propriétés__ de toute ressource du dossier.
1. Accédez à l’onglet __Avancé__
1. Vérifiez la valeur de la propriété mise à jour, par exemple __Copyright__ mappé à la propriété `metadata/dc:rights` JCR mise à jour, qui reflète désormais la valeur fournie dans le paramètre `propertyValue`, par exemple __Utilisation limitée WKND__

![Mise à jour des métadonnées d’utilisation restreinte WKND](./assets/service-credentials/asset-metadata.png)

## Félicitations ! 

Maintenant que nous avons accédé par programmation à AEM en tant que Cloud Service à l’aide d’un jeton d’accès au développement local, ainsi que d’un jeton d’accès service à service prêt pour la production !

