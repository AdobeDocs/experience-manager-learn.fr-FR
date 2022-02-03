---
title: Informations d’identification du service
description: Les informations d’identification du service AEM sont utilisées pour faciliter les applications, systèmes et services externes permettant d’interagir par programmation avec les services AEM Author ou Publish via HTTP.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: ef4579a44c1c940a3b7441e336db3790a0c7afd7
workflow-type: tm+mt
source-wordcount: '1901'
ht-degree: 0%

---

# Informations d’identification du service

Les intégrations à AEM as a Cloud Service doivent pouvoir s’authentifier en toute sécurité dans AEM. AEM Developer Console accorde l’accès aux informations d’identification du service, qui sont utilisées pour faciliter les applications, systèmes et services externes pour interagir par programmation avec les services AEM Author ou Publish sur HTTP.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Les informations d’identification du service peuvent apparaître similaires. [Jetons d’accès au développement local](./local-development-access-token.md) mais sont différents de plusieurs manières clés :

+ Les informations d’identification du service sont _not_ les jetons d’accès ; il s’agit plutôt d’informations d’identification utilisées pour _obtenir_ jetons d’accès.
+ Les informations d’identification du service sont plus permanentes (expirent tous les 365 jours) et ne changent pas, sauf si elles sont révoquées, tandis que les jetons d’accès au développement local expirent tous les jours.
+ Les informations d’identification du service pour un environnement as a Cloud Service AEM sont mappées à un utilisateur de compte technique unique AEM, tandis que les jetons d’accès au développement local s’authentifient en tant qu’utilisateur  ayant généré le jeton d’accès.
+ Un environnement AEM as a Cloud Service comporte une information d’identification de service qui correspond à un compte technique AEM utilisateur. Les informations d’identification du service ne peuvent pas être utilisées pour s’authentifier dans le même environnement as a Cloud Service AEM que des utilisateurs AEM compte technique différents.

Les informations d’identification du service et les jetons d’accès qu’ils génèrent, ainsi que les jetons d’accès au développement local, doivent être gardés secrets, car les trois peuvent être utilisés pour accéder à leurs environnements as a Cloud Service respectifs AEM

## Générer les informations d’identification du service

La génération des informations d’identification du service est divisée en deux étapes :

1. Initialisation ponctuelle des informations d’identification du service par un administrateur de l’organisation Adobe IMS
1. Téléchargement et utilisation des informations d’identification du service JSON

### Initialisation des informations d’identification du service

Contrairement aux jetons d’accès de développement locaux, les informations d’identification du service requièrent un _initialisation unique_ par l’administrateur IMS de votre organisation d’Adobe avant de pouvoir les télécharger.

![Initialisation des informations d’identification du service](assets/service-credentials/initialize-service-credentials.png)

__Il s’agit d’une initialisation unique par environnement as a Cloud Service AEM__

1. Vérifiez que vous êtes connecté en tant que :
   + Votre administrateur de l’organisation Adobe IMS
   + Membre du __Cloud Manager - Développeur__ Profil de produit IMS
   + Membre du __Utilisateur AEM__ ou __Administrateurs AEM__ Profil de produit IMS sur __Auteur AEM__
1. Connectez-vous à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Ouvrez le programme contenant l’environnement as a Cloud Service AEM pour intégrer la configuration des informations d’identification du service pour
1. Appuyez sur les points de suspension en regard de l’environnement dans la __Environnements__ et sélectionnez __Developer Console__
1. Appuyez sur __Intégrations__ tab
1. Appuyer __Obtention des informations d’identification du service__ button
1. Les informations d’identification du service seront initialisées et affichées au format JSON.

![AEM Developer Console - Intégrations - Obtention des informations d’identification du service](./assets/service-credentials/developer-console.png)

Une fois l’AEM comme informations d’identification de service de l’environnement de Cloud Service initialisé, d’autres développeurs AEM de votre organisation Adobe IMS peuvent les télécharger.

### Téléchargement des informations d’identification du service

![Téléchargement des informations d’identification du service](assets/service-credentials/download-service-credentials.png)

Le téléchargement des informations d’identification du service suit les mêmes étapes que l’initialisation. Si l’initialisation n’a pas encore eu lieu, une erreur s’affiche lorsque l’utilisateur appuie sur la balise __Obtention des informations d’identification du service__ bouton .

1. Vérifiez que vous êtes connecté en tant que :
   + Membre du __Cloud Manager - Développeur__ Profil de produit IMS (qui accorde l’accès à AEM Developer Console)
      + Les environnements Sandbox AEM as a Cloud Service ne nécessitent pas cette  __Cloud Manager - Développeur__ abonnement
   + Membre du __Utilisateur AEM__ ou __Administrateurs AEM__ Profil de produit IMS sur __Auteur AEM__
1. Connectez-vous à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Ouvrez le programme contenant l’environnement as a Cloud Service AEM à intégrer à
1. Appuyez sur les points de suspension en regard de l’environnement dans la __Environnements__ et sélectionnez __Developer Console__
1. Appuyez sur __Intégrations__ tab
1. Appuyer __Obtention des informations d’identification du service__ button
1. Appuyez sur le bouton de téléchargement dans le coin supérieur gauche pour télécharger le fichier JSON contenant la valeur Informations d’identification du service, puis enregistrez le fichier à un emplacement sécurisé.
   + _Si les informations d’identification du service sont compromises, contactez immédiatement l’assistance Adobe pour qu’elles soient révoquées._

## Installation des informations d’identification du service

Les informations d’identification du service fournissent les détails nécessaires à la génération d’un jeton JWT, qui est échangé contre un jeton d’accès utilisé pour s’authentifier avec AEM as a Cloud Service. Les informations d’identification du service doivent être stockées dans un emplacement sécurisé accessible par les applications, systèmes ou services externes qui l’utilisent pour accéder à AEM. Le mode et l’emplacement de gestion des informations d’identification du service seront uniques par client.

Pour plus de simplicité, ce tutoriel transmet les informations d’identification du service dans via la ligne de commande. Toutefois, travaillez avec votre équipe de sécurité informatique pour comprendre comment stocker ces informations d’identification et y accéder conformément aux directives de sécurité de votre entreprise.

1. Copiez le [téléchargé JSON Informations d’identification du service.](#download-service-credentials) dans un fichier nommé `service_token.json` à la racine du projet
   + Mais rappelez-vous, ne validez jamais vos identifiants à Git !

## Utilisation des informations d’identification du service

Les informations d’identification du service, un objet JSON entièrement formé, ne sont pas identiques au JWT ni au jeton d’accès. Les informations d’identification du service (qui contiennent une clé privée) sont utilisées pour générer un jeton d’accès JWT qui est échangé avec les API Adobe IMS.

![Informations d’identification du service - Application externe](assets/service-credentials/service-credentials-external-application.png)

1. Téléchargez les informations d’identification du service depuis AEM Developer Console vers un emplacement sécurisé.
1. Une application externe doit interagir par programmation avec AEM environnements as a Cloud Service.
1. L’application externe lit les informations d’identification du service à partir d’un emplacement sécurisé.
1. L’application externe utilise les informations d’identification du service pour construire un jeton JWT.
1. Le jeton JWT est envoyé à Adobe IMS pour échanger contre un jeton d’accès.
1. Adobe IMS renvoie un jeton d’accès qui peut être utilisé pour accéder à AEM as a Cloud Service
   + Un délai d’expiration peut être demandé pour les jetons d’accès. Il est préférable de raccourcir la durée de vie du jeton d’accès et de l’actualiser si nécessaire.
1. L’application externe effectue des requêtes HTTP pour AEM as a Cloud Service, en ajoutant le jeton d’accès en tant que jeton porteur à l’en-tête d’autorisation des requêtes HTTP.
1. AEM as a Cloud Service reçoit la requête HTTP, authentifie la requête et effectue le travail demandé par la requête HTTP, puis renvoie une réponse HTTP à l’application externe.

### Mises à jour de l’application externe

Pour accéder à AEM as a Cloud Service à l’aide des informations d’identification du service, votre application externe doit être mise à jour de trois façons :

1. Lecture dans les informations d’identification du service
   + Pour plus de simplicité, nous lirons ces informations à partir du fichier JSON téléchargé. Toutefois, dans les scénarios d’utilisation réelle, les informations d’identification du service doivent être stockées en toute sécurité conformément aux directives de sécurité de votre entreprise.
1. Génération d’un JWT à partir des informations d’identification du service
1. Échange du JWT pour un jeton d’accès
   + Lorsque les informations d’identification du service sont présentes, notre application externe utilise ce jeton d’accès au lieu du jeton d’accès au développement local lors de l’accès AEM as a Cloud Service

Dans ce tutoriel, Adobe `@adobe/jwt-auth` Le module npm est utilisé pour (1) générer le JWT à partir des informations d’identification du service et (2) l’échanger contre un jeton d’accès, dans un seul appel de fonction. Si votre application n’est pas basée sur JavaScript, veuillez consulter la section [exemple de code dans d’autres langues](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) pour savoir comment créer un jeton JWT à partir des informations d’identification du service et l’échanger contre un jeton d’accès avec Adobe IMS.

## Lire les informations d’identification du service

Consultez la section `getCommandLineParams()` et vérifiez que nous pouvons lire dans les fichiers JSON Informations d’identification du service en utilisant le même code que celui utilisé pour lire dans le JSON JSON Jeton d’accès au développement local.

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

Une fois les informations d’identification du service lues, elles sont utilisées pour générer un jeton d’accès JWT qui est ensuite échangé avec les API Adobe IMS pour un jeton d’accès qui peut ensuite être utilisé pour accéder à AEM as a Cloud Service.

Cet exemple d’application est basé sur Node.js. Il est donc préférable d’utiliser [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) module npm pour faciliter la génération (1) JWT et l’échange (20) avec Adobe IMS. Si votre application est développée à l’aide d’une autre langue, veuillez consulter [les exemples de code appropriés ;](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) sur la manière de construire la requête HTTP vers Adobe IMS à l’aide d’autres langages de programmation.

1. Mettez à jour le `getAccessToken(..)` pour examiner le contenu du fichier JSON et déterminer s’il représente un jeton d’accès au développement local ou des informations d’identification du service. Pour ce faire, il suffit de vérifier l’existence de la variable `.accessToken` qui n’existe que pour le JSON JSON du jeton d’accès au développement local.

   Si les informations d’identification du service sont fournies, l’application génère un JWT et l’échange avec Adobe IMS pour un jeton d’accès. Nous utiliserons la variable [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)&#39;s `auth(...)` qui génère un JWT et l’échange pour un jeton d’accès dans un seul appel de fonction.  Les paramètres de `auth(..)` est un [Objet JSON composé d’informations spécifiques](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) disponible à partir du JSON des informations d’identification du service, comme décrit ci-dessous dans le code .

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

   Désormais, selon le fichier JSON transmis par l’intermédiaire de : JSON du jeton d’accès au développement local ou JSON des informations d’identification du service. `file` paramètre de ligne de commande, l’application dérive un jeton d’accès.

   N’oubliez pas que même si les informations d’identification du service expirent tous les 365 jours, le jeton d’accès JWT et le jeton d’accès correspondant expirent fréquemment et doivent être actualisés avant expiration. Pour ce faire, utilisez une `refresh_token` [fourni par Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Une fois ces modifications en place, et le fichier JSON des informations d’identification du service téléchargé depuis AEM Developer Console (et, pour plus de simplicité, enregistré sous `service_token.json` le même dossier que celui-ci `index.js`), exécutez l’application en remplaçant le paramètre de ligne de commande `file` avec `service_token.json`, puis mettez à jour la variable `propertyValue` à une nouvelle valeur afin que les effets soient visibles dans AEM.

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

   Le __403 - Interdit__ lignes, indiquez les erreurs dans les appels API HTTP à AEM as a Cloud Service. Ces erreurs 403 interdites se produisent lors de la tentative de mise à jour des métadonnées des ressources.

   Cela s’explique par le fait que le jeton d’accès dérivé des informations d’identification du service authentifie la demande d’AEM à l’aide d’un compte technique créé automatiquement AEM utilisateur qui, par défaut, dispose uniquement d’un accès en lecture. Pour permettre l’accès en écriture de l’application à AEM, l’utilisateur du compte technique AEM associé au jeton d’accès doit se voir accorder l’autorisation dans l’.

## Configurer l’accès dans AEM

Le jeton d’accès dérivé des informations d’identification du service utilise un compte technique AEM utilisateur membre du groupe d’utilisateurs AEM contributeurs .

![Informations d’identification du service - Utilisateur AEM compte technique](./assets/service-credentials/technical-account-user.png)

Une fois que le compte technique AEM utilisateur existe dans AEM (après la première requête HTTP avec le jeton d’accès), les autorisations de cet utilisateur peuvent être gérées de la même manière que les autres utilisateurs .

1. Tout d’abord, recherchez le nom de connexion AEM du compte technique en ouvrant le fichier JSON des informations d’identification du service téléchargé à partir d’AEM Developer Console, puis recherchez le `integration.email` qui doit ressembler à : `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Connectez-vous au service Auteur de l’environnement AEM correspondant en tant qu’administrateur AEM
1. Accédez à __Outils__ > __Sécurité__ > __Utilisateurs__
1. Localisez l’utilisateur AEM avec le __Nom de connexion__ identifié à l’étape 1 et ouvrez le __Propriétés__
1. Accédez au __Groupes__ et ajoutez le __Utilisateurs de DAM__ groupe (qui écrit l’accès aux ressources)
1. Appuyer __Enregistrer et fermer__

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

1. Connectez-vous à l’environnement as a Cloud Service AEM qui a été mis à jour (en utilisant le même nom d’hôte que celui fourni dans la variable `aem` paramètre de ligne de commande)
1. Accédez au __Ressources__ > __Fichiers__
1. Accédez au dossier de ressources spécifié par la propriété `folder` paramètre de ligne de commande, par exemple __WKND__ > __Anglais__ > __Aventures__ > __Goûts du vin de Napa__
1. Ouvrez le __Propriétés__ pour toute ressource du dossier
1. Accédez au __Avancé__ tab
1. Vérifiez la valeur de la propriété mise à jour, par exemple __Copyright__ qui est mappé à la mise à jour `metadata/dc:rights` Propriété JCR, qui reflète désormais la valeur fournie dans la propriété `propertyValue` par exemple, __Utilisation restreinte de WKND__

![Mise à jour des métadonnées d’utilisation restreinte WKND](./assets/service-credentials/asset-metadata.png)

## Félicitations !

Maintenant que nous avons accédé par programmation à AEM as a Cloud Service à l’aide d’un jeton d’accès au développement local, ainsi que d’un jeton d’accès service à service prêt pour la production !
