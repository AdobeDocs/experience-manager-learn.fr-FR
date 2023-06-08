---
title: Vérification d’une extension d’interface utilisateur AEM
description: Découvrez comment prévisualiser, tester et vérifier une extension de l’interface utilisateur d’AEM avant son déploiement en production.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 0%

---

# Vérification d’une extension

Les extensions de l’interface utilisateur d’AEM peuvent être vérifiées par rapport à n’importe quel environnement as a Cloud Service dans l’organisation d’Adobe à laquelle appartient l’extension.

Le test d’une extension est effectué par le biais d’une URL spécialement conçue qui indique à AEM de charger l’extension, uniquement pour cette requête.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> La vidéo ci-dessus montre l’utilisation d’une extension de la console de fragments de contenu pour illustrer l’aperçu et la vérification de l’application de l’extension App Builder. Toutefois, il est important de noter que les concepts couverts peuvent être appliqués à toutes les extensions de l’interface utilisateur d’AEM.

## URL de l’interface utilisateur AEM

![AEM URL de la console de fragments de contenu](./assets/verify/content-fragment-console-url.png){align="center"}

Pour créer une URL qui monte l’extension hors production dans AEM, l’URL de l’interface utilisateur d’AEM dans laquelle l’extension est injectée doit être obtenue. Accédez à l’environnement as a Cloud Service AEM pour vérifier l’extension et ouvrez l’interface utilisateur dans laquelle l’extension doit être prévisualisée.

Par exemple, pour prévisualiser une extension pour la console Fragment de contenu :

1. Connectez-vous à l’AEM as a Cloud Service souhaitée.
2. Sélectionnez la __Fragments de contenu__ icône .
3. Attendez que la console de fragments de contenu AEM se charge dans le navigateur.
4. Copiez l’URL de la console de fragments de contenu AEM dans la barre d’adresse du navigateur. Elle doit ressembler à :

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Cette URL est utilisée ci-dessous lors de la conception des URL pour le développement et la vérification des étapes. Si vous vérifiez l’extension par rapport à d’autres interfaces utilisateur AEM, obtenez ces URL et appliquez les mêmes étapes que ci-dessous.

## Vérifier les versions de développement locales

1. Ouvrez une ligne de commande à la racine du projet d’extension.
1. Exécution de l’extension d’interface utilisateur AEM en tant qu’application App Builder locale

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

1. Ajoutez les deux paramètres de requête suivants au [URL de l’interface utilisateur d’AEM](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, généralement `&ext=https://localhost:9080`.

   Ajoutez les deux paramètres de requête ci-dessus (`devMode` et `ext`) en tant que __first__ paramètres de requête dans l’URL. AEM l’interface utilisateur extensible utilise des itinéraires de hachage (`#/@wknd/aem/...`), corrigez de manière incorrecte les paramètres après l’événement `#` ne fonctionne pas.

   L’URL d’aperçu doit se présenter comme suit :

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

2. Copiez et collez l’URL d’aperçu dans votre navigateur.

   + Vous devrez peut-être commencer, puis périodiquement, [accepter le certificat HTTPS ;](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) pour l’hôte de l’application locale (`https://localhost:9080`).

3. L’interface utilisateur AEM se charge avec la version locale de l’extension qui y est injectée pour vérification.

>[!IMPORTANT]
>
>Souvenez-vous que lorsque vous utilisez cette approche, l’extension en cours de développement n’affecte que votre expérience, et que tous les autres utilisateurs de l’interface utilisateur AEM expérimentent l’interface utilisateur sans l’extension injectée.

## Vérifier les versions d’étape

1. Ouvrez une ligne de commande à la racine du projet d’extension.
1. Assurez-vous que l’espace de travail de test est principal (ou que l’espace de travail est utilisé à des fins de vérification).

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

1. Ajoutez les deux paramètres de requête suivants au [URL de l’interface utilisateur d’AEM](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Ajoutez les deux paramètres de requête ci-dessus (`devMode` et `ext`) en tant que __first__ les paramètres de requête dans l’URL, car les interfaces AEM extensibles utilisent un itinéraire de hachage (`#/@wknd/aem/...`), corrigez de manière incorrecte les paramètres après l’événement `#` ne fonctionne pas.

   L’URL d’aperçu doit se présenter comme suit :

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Copiez et collez l’URL d’aperçu dans votre navigateur.
1. La console AEM de fragments de contenu injecte la version de l’extension déployée dans l’espace de travail Stage (évaluation) dans . Cette URL d’évaluation peut être partagée avec les utilisateurs d’assurance qualité ou d’entreprise à des fins de vérification.

N’oubliez pas que lorsque vous utilisez cette approche, l’extension intermédiaire n’est injectée que dans AEM de la console de fragments de contenu lorsque l’accès avec l’URL d’évaluation de l’appareil est autorisé.

1. Les extensions déployées peuvent être mises à jour en exécutant `aio app deploy` et ces modifications se répercutent automatiquement lors de l’utilisation de l’URL d’aperçu.
1. Pour supprimer une extension à vérifier, exécutez `aio app undeploy`.

## Aperçu du signet d’applet

Pour faciliter la création des URL d’aperçu et de prévisualisation décrites ci-dessus, un signet d’applet JavaScript qui charge l’extension peut être créé.

Le signet d’applet ci-dessous prévisualise la variable [versions de développement locales](#verify-local-development-builds) de l’extension sur `https://localhost:9080`. Pour prévisualiser [versions d’étape](#verify-stage-builds), créez un signet d’applet à l’aide de la fonction `previewApp` définie sur l’URL de l’application App Builder déployée.

1. Créez un signet dans votre navigateur.
2. Modifiez le signet.
3. Attribuez un nom significatif à un signet, tel que `AEM UI Extension Preview (localhost:9080)`.
4. Définissez l’URL du signet sur le code suivant :

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

5. Accédez à une interface utilisateur AEM extensible pour charger l’extension d’aperçu, puis cliquez sur le signet d’applet.

>[!TIP]
>
> Si l’extension App Builder ne se charge pas, lors de l’utilisation de : `&ext=https://localhost:9080`, ouvrez cet hôte et ce port directement dans un onglet de navigateur et acceptez le certificat auto-signé. Essayez à nouveau le signet d’applet.
