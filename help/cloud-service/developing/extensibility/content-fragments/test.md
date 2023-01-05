---
title: Test d’une extension de console de fragments de contenu AEM
description: Découvrez comment tester une extension de console de fragments de contenu AEM avant son déploiement en production.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: fbc8c11841f5b5e04a99ba74fac6f01dc3e3a2da
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Test d’une extension

AEM les extensions de la console de fragments de contenu peuvent être testées par rapport à n’importe quel environnement as a Cloud Service dans l’organisation d’Adobe à laquelle appartient l’extension.

Le test d’une extension s’effectue par le biais d’une URL spécialement conçue qui indique à la console AEM fragment de contenu de charger l’extension.

>[!VIDEO](https://video.tv.adobe.com/v/3412877/?quality=12&learn=on)

## AEM URL de la console de fragments de contenu

![AEM URL de la console de fragments de contenu](./assets/test/content-fragment-console-url.png){align="center"}

Pour créer une URL qui monte l’extension hors production dans une console de fragments de contenu AEM, l’URL de la console AEM fragment de contenu souhaitée doit être collectée. Accédez à l’environnement as a Cloud Service AEM pour tester l’extension et copiez l’URL de sa console de fragments de contenu AEM.

1. Connectez-vous à l’AEM as a Cloud Service souhaitée.

   + Utilisation d’un environnement de développement AEM pour [test des versions de développement](#testing-development-builds)
   + Utilisation de l’environnement d’évaluation AEM ou de développement d’AQ pour [test des versions d’étape](#testing-stage-builds)

1. Sélectionnez la __Fragments de contenu__ icône .
1. Attendez que la console de fragments de contenu AEM se charge dans le navigateur.
1. Copiez l’URL de la console de fragments de contenu AEM dans la barre d’adresse du navigateur. Elle doit ressembler à :

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Cette URL est utilisée ci-dessous lors de la conception des URL pour le développement et les tests d’évaluation.

## Test des versions de développement local

1. Ouvrez une ligne de commande à la racine du projet d’extension.
1. Exécutez l’extension AEM Content Fragment Console en tant qu’application locale App Builder

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Prenez note de l’URL de l’application locale, comme illustré ci-dessus : `-> https://localhost:9080`

1. Ajoutez les deux paramètres de requête suivants au [URL d’AEM de la console de fragments de contenu](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, généralement `&ext=https://localhost:9080`.

   Ajoutez les deux paramètres de requête ci-dessus (`devMode` et `ext`) en tant que __first__ paramètres de requête dans l’URL, car la console de fragments de contenu utilise un itinéraire de hachage (`#/@wknd/aem/...`), corrigez de manière incorrecte les paramètres après l’événement `#` ne fonctionnera pas.

   L’URL de test doit se présenter comme suit :

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copiez et collez l’URL de test dans votre navigateur.

   + Vous devrez peut-être commencer, puis périodiquement, vous devrez [accepter le certificat HTTPS ;](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) pour l’hôte de l’application locale (`https://localhost:9080`).

1. La console de fragments de contenu AEM se charge avec la version locale de l’extension qui y est injectée à des fins de test et les modifications à chaud sont appliquées tant que l’application du créateur d’applications locale est en cours d’exécution.

>[!IMPORTANT]
>
>N’oubliez pas que lorsque vous utilisez cette approche, l’extension en cours de développement n’affecte que votre expérience. Tous les autres utilisateurs de la console Fragment de contenu AEM y accèdent sans l’extension injectée.


## Versions de l’étape de test

1. Ouvrez une ligne de commande à la racine du projet d’extension.
1. Assurez-vous que l’espace de travail intermédiaire est principal (ou tout espace de travail utilisé à des fins de test).

   ```shell
   $ aio app use -w Stage
   ```
   Fusionner toutes les modifications dans `.env` et `.aio`.
1. Déployez l’extension mise à jour de l’application App Builder. S’il n’est pas connecté, exécutez `aio login` en premier.

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. Ajoutez les deux paramètres de requête suivants au [URL d’AEM de la console de fragments de contenu](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Ajoutez les deux paramètres de requête ci-dessus (`devMode` et `ext`) en tant que __first__ paramètres de requête dans l’URL, car la console de fragments de contenu utilise un itinéraire de hachage (`#/@wknd/aem/...`), corrigez de manière incorrecte les paramètres après l’événement `#` ne fonctionnera pas.

   L’URL de test doit se présenter comme suit :

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copiez et collez l’URL de test dans votre navigateur.
1. La console AEM de fragments de contenu injecte la version de l’extension déployée dans l’espace de travail Stage (évaluation) dans . Cette URL d’évaluation peut être partagée avec les utilisateurs d’assurance qualité ou d’entreprise à des fins de test.

N’oubliez pas que lorsque vous utilisez cette approche, l’extension intermédiaire n’est injectée que dans AEM de la console de fragments de contenu lorsque l’accès avec l’URL d’évaluation de l’appareil est autorisé.

1. Les extensions déployées peuvent être mises à jour en exécutant `aio app deploy` et ces modifications se répercutent automatiquement lors de l’utilisation de l’URL de test.
1. Pour supprimer une extension à des fins de test, exécutez `aio app undeploy`.



