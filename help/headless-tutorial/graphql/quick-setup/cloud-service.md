---
title: Configuration rapide d’AEM Headless pour AEM as a Cloud Service
description: La configuration rapide d’AEM Headless permet de se familiariser avec AEM Headless en utilisant le contenu de l’exemple de projet WKND Site et une application React qui exploite le contenu via les API GraphQL d’AEM Headless.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 850
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 100%

---

# Configuration rapide d’AEM Headless pour AEM as a Cloud Service

La configuration rapide d’AEM Headless permet de se familiariser avec AEM Headless grâce au contenu de l’exemple de projet WKND Site et à un exemple d’application React (une SPA) qui exploite le contenu via les API GraphQL d’AEM Headless.

## Prérequis

Les éléments suivants sont nécessaires pour effectuer cette configuration rapide :

+ Un environnement sandbox AEM as a Cloud Service (de préférence de développement).
+ Un accès à AEM as a Cloud Service et Cloud Manager.
   + Un accès d’__administration AEM__ à AEM as a Cloud Service.
   + Un accès de __gestionnaire déploiement de Cloud Manager__ à Cloud Manager.
+ Les outils suivants doivent être installés localement :
   + [Node.js v18](https://nodejs.org/fr/)
   + [Git](https://git-scm.com/)
   + Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/)).

## 1. Créer un référentiel Git Cloud Manager

Créez tout d’abord un référentiel Git Cloud Manager pour déployer WKND Site. WKND Site est un exemple de projet de site web AEM avec du contenu (des fragments de contenu) et un point d’entrée GraphQL d’AEM utilisé par l’application React de la configuration rapide.

_Capture vidéo des étapes._
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Accédez à [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com).
1. Sélectionnez le __Programme__ de Cloud Manager contenant l’environnement AEM as a Cloud Service à utiliser pour cette configuration rapide.
1. Créez un référentiel Git pour le projet WKND Site.
   1. Sélectionnez __Référentiels__ dans le menu de navigation supérieur.
   1. Sélectionnez __Ajouter un référentiel__ dans la barre d’actions supérieure.
   1. Nommez le nouveau référentiel Git : `aem-headless-quick-setup-wknd`.
      + Les noms de référentiel Git doivent être uniques pour chaque organisation d’Adobe.
   1. Sélectionnez __Enregistrer__, et attendez que le référentiel Git soit initialisé.

## 2. Transférer l’exemple de projet WKND Site vers le référentiel Git de Cloud Manager

Une fois le référentiel Git de Cloud Manager créé, clonez le code source du projet WKND Site à partir de GitHub et envoyez-le au référentiel Git de Cloud Manager. Cloud Manager peut désormais accéder au projet WKND Site et le déployer dans l’environnement AEM as a Cloud Service.

_Capture vidéo des étapes._
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. À partir de la ligne de commande, clonez l’exemple de code source du projet WKND Site à partir de GitHub.

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Ajoutez le référentiel Git de Cloud Manager en tant que référentiel distant.
   1. Sélectionnez __Référentiels__ dans le menu de navigation supérieur.
   1. Sélectionnez __Accéder aux informations sur le référentiel__ à partir de la barre d’actions supérieure.
   1. Exécutez la commande trouvée dans __Ajouter un élément distant à votre référentiel Git__ à partir de la ligne de commande.

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Envoyez l’exemple de code source du projet de votre référentiel Git local vers le référentiel Git de Cloud Manager.

   ```shell
   $ git push adobe main:main
   ```

   Lorsqu’on vous invite à fournir des informations d’identification, indiquez le __Nom d’utilisateur__ et le __Mot de passe__ de la boîte de dialogue modale __Informations sur le référentiel__ de Cloud Manager.

## 3. Déployer WKND Site vers AEM as a Cloud Service

Une fois le projet WKND Site transmis au référentiel Git de Cloud Manager, il ne peut pas être déployé sur AEM as a Cloud Service avec les pipelines de Cloud Manager.

Gardez à l’esprit que le projet WKND Site fournit un exemple de contenu que l’application React exploite grâce aux API GraphQL d’AEM Headless.

_Capture vidéo des étapes._
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Joindre un __Pipeline de déploiement hors production__ au nouveau référentiel Git
   1. Sélectionnez __Pipelines__ dans le menu de navigation supérieur.
   1. Sélectionnez __Ajouter un pipeline__ à partir de la barre d’actions supérieure.
   1. Dans l’onglet __Configuration__ :
      1. Sélectionnez l’option __Pipeline de déploiement__.
      1. Définissez le __Nom du pipeline hors production__ sur `Dev Deployment pipeline`.
      1. Sélectionnez __Déclencheur de déploiement > Lors des modifications Git__.
      1. Sélectionnez __Comportement en cas d’échecs de mesures importants > Continuer immédiatement__.
      1. Sélectionnez __Continuer__.
   1. Dans l’onglet __Code source__ :
      1. Sélectionnez l’option __Code de pile pleine__.
      1. Sélectionnez l’__environnement de développement AEM as a Cloud Service__ dans la zone de sélection __Environnements de déploiement éligibles__.
      1. Sélectionnez `aem-headless-quick-setup-wknd` dans la zone de sélection __Référentiel__.
      1. Sélectionnez `main` dans la zone de sélection __Branche Git__.
      1. Sélectionnez __Enregistrer__.
1. Exécuter le __Pipeline de déploiement de développement__
   1. Sélectionnez __Overview__ dans la barre de navigation supérieure.
   1. Localisez le __Pipeline de déploiement de développement__ que vous venez de créer dans la section __Pipelines__.
   1. Sélectionnez les points de suspension __...__ à droite de l’entrée du pipeline.
   1. Sélectionnez __Exécuter__ et confirmez l’action dans la boîte de dialogue modale.
   1. Sélectionnez les points de suspension __...__ à droite du pipeline maintenant en cours d’exécution.
   1. Sélectionnez __Afficher les détails__.
1. Dans la fenêtre des détails de l’exécution du pipeline, surveillez la progression de l’exécution du pipeline jusqu’à ce qu’elle soit terminée. L’exécution du pipeline dure généralement entre 30 et 40 minutes.

## 4. Télécharger et exécuter l’application React WKND

Une fois AEM as a Cloud Service amorcé avec le contenu du projet WKND Site, téléchargez et démarrez l’exemple d’application React WKND, qui accède au contenu de WKND Site par le biais des API GraphQL d’AEM Headless.

_Capture vidéo des étapes._
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. À partir de la ligne de commande, clonez le code source de l’application React à partir de GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Ouvrez le dossier `~/Code/aem-guides-wknd-graphql/react-app` dans votre IDE.
1. Dans l’IDE, ouvrez le fichier `.env.development`.
1. Recherchez l’URI hôte du service de __publication__ AEM as a Cloud Service dans la propriété `REACT_APP_HOST_URI`.

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Pour trouver l’URI hôte de votre service de publication AEM as a Cloud Service, procédez comme suit :

   1. Dans Cloud Manager, sélectionnez __Environnements__ dans la barre de navigation supérieure.
   1. Sélectionnez l’environnement de __Développement__.
   1. Recherchez le lien __Service de publication (AEM et Dispatcher)__ dans le tableau __Segments d’environnement__.
   1. Copiez l’adresse du lien et utilisez-la comme URI du service de publication AEM as a Cloud Service.

1. Dans l’IDE, enregistrez les modifications apportées à `.env.development`.
1. À partir de la ligne de commande, exécutez l’application React.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. L’application React, qui s’exécute localement, démarre à l’adresse [http://localhost:3000](http://localhost:3000) et affiche une liste d’Adventures provenant d’AEM as a Cloud Service, à l’aide des API GraphQL d’AEM Headless.

## 5. Modifier le contenu dans AEM

Maintenant que l’exemple d’application React WKND peut se connecter aux API GraphQL d’AEM Headless et utiliser le contenu du site, vous pouvez commencer à créer du contenu dans le service de création AEM et voir comment celui-ci s’affiche dans l’application React.

_Capture vidéo des étapes._
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Connectez-vous au service de création AEM as a Cloud Service.
1. Accédez à __Ressources > Fichiers > WKND Shared > Français > Adventures__.
1. Ouvrez le dossier __Cycling Southern Utah__.
1. Sélectionnez le fragment de contenu __Cycling Southern Utah__, puis cliquez sur __Modifier__ dans la barre d’actions supérieure.
1. Mettez à jour certains champs du fragment de contenu, par exemple :
   + Titre : `Cycling Utah's National Parks`.
   + Trip Length : `6 Days`
   + Difficulty : `Intermediate`
   + Price : `3500`.
   + Image principale : `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Cliquez sur __Enregistrer__ dans la barre d’actions supérieure.
1. Sélectionnez __Publication rapide__ dans les points de suspension __...__ de la barre d’actions supérieure.
1. Actualisez l’application React en cours d’exécution à l’adresse [http://localhost:3000](http://localhost:3000).
1. Dans l’application React, sélectionnez l’Adventure Cycling mise à jour, puis vérifiez les modifications apportées au contenu du fragment de contenu.

1. En utilisant la même approche, dans le service de création AEM, effectuez les opérations suivantes :
   1. Annulez la publication d’un fragment de contenu d’Adventure existant et vérifiez qu’il n’apparaît plus dans l’application React.
   1. Créez et publiez un fragment de contenu d’Adventure, puis vérifiez qu’il apparaît bien dans l’application React.

   >[!TIP]
   >
   > Pour en savoir plus sur la création et la publication de fragments de contenu ou l’annulation de leur publication, regardez la capture vidéo ci-dessus.

## Félicitations.

Félicitations. Vous avez utilisé AEM Headless pour alimenter une application React.

Pour comprendre en détail comment l’application React exploite du contenu d’AEM as a Cloud Service, consultez le [Tutoriel AEM Headless](../multi-step/overview.md). Ce tutoriel explique comment les fragments de contenu sont créés dans AEM et comment cette application React exploite leur contenu au format JSON.

### Étapes suivantes

+ [Démarrer le tutoriel d’AEM Headless](../multi-step/overview.md)
