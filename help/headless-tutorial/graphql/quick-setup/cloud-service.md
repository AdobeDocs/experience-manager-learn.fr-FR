---
title: Configuration rapide AEM sans affichage pour AEM as a Cloud Service
description: La configuration rapide AEM sans affichage vous permet d’utiliser AEM sans affichage à l’aide du contenu de l’exemple de projet WKND Site et d’une application React qui consomme le contenu via les API GraphQL sans affichage.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 2%

---

# Configuration rapide AEM sans affichage pour AEM as a Cloud Service

La configuration rapide AEM sans affichage vous permet d’utiliser AEM sans affichage à l’aide du contenu de l’exemple de projet de site WKND, ainsi qu’un exemple d’application React (a SPA) qui consomme le contenu via les API GraphQL sans affichage .

## Prérequis

Les éléments suivants sont nécessaires pour suivre cette configuration rapide :

+ AEM environnement Sandbox as a Cloud Service (de préférence Développement)
+ Accès à AEM as a Cloud Service et Cloud Manager
   + __Administrateur AEM__ accès à AEM as a Cloud Service
   + __Cloud Manager - Gestionnaire de déploiement__ accès à Cloud Manager
+ Les outils suivants doivent être installés localement :
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + Un IDE (par exemple, [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Création d’un référentiel Git Cloud Manager

Créez tout d’abord un référentiel Git Cloud Manager utilisé pour déployer le site WKND. Le site WKND est un exemple de projet AEM site web qui contient du contenu (des fragments de contenu) et un point de terminaison GraphQL AEM utilisé par l’application React de la configuration rapide.

_Arrêt sur image des étapes_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. Accédez à [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Sélectionnez Cloud Manager . __Programme__ contenant l’environnement as a Cloud Service AEM à utiliser pour cette configuration rapide
1. Création d’un référentiel Git pour le projet de site WKND
   1. Sélectionner __Référentiels__ dans la navigation supérieure ;
   1. Sélectionner __Ajouter un référentiel__ dans la barre d’actions supérieure
   1. Nommez le nouveau référentiel Git : `aem-headless-quick-setup-wknd`
      + Les noms de référentiel Git doivent être uniques par organisation d’Adobe,
   1. Sélectionner __Enregistrer__, et attendez que le référentiel Git soit initialisé.

## 2. Transférez l’exemple de projet de site WKND vers le référentiel Git de Cloud Manager.

Une fois le référentiel Git de Cloud Manager créé, clonez le code source du projet WKND Site à partir de GitHub et envoyez-le au référentiel Git de Cloud Manager. Cloud Manager permet désormais d’accéder au projet WKND Site et de le déployer dans l’environnement as a Cloud Service AEM.

_Arrêt sur image des étapes_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. À partir de la ligne de commande, clonez l’exemple de code source du projet WKND Site à partir de GitHub.

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Ajout du référentiel Git de Cloud Manager en tant que référentiel distant
   1. Sélectionner __Référentiels__ dans la navigation supérieure ;
   1. Sélectionner __Accès aux informations sur le référentiel__ à partir de la barre d’actions supérieure
   1. Exécuter la commande trouvée dans __Ajout d’un élément distant à votre référentiel Git__ à partir de la ligne de commande

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. Envoyez l’exemple de code source du projet de votre référentiel Git local vers le référentiel Git de Cloud Manager.

   ```shell
   $ git push adobe main:main
   ```

   Lorsque vous y êtes invité, saisissez la variable __Nom d’utilisateur__ et __Mot de passe__ de Cloud Manager __Informations sur le référentiel__ modale.

## 3. Déployez le site WKND pour AEM as a Cloud Service

Une fois le projet de site WKND transmis au référentiel Git de Cloud Manager, il ne peut pas être déployé sur AEM as a Cloud Service à l’aide des pipelines de Cloud Manager.

Gardez à l’esprit que le projet de site WKND fournit un exemple de contenu que l’application React utilise sur AEM API GraphQL sans affichage.

_Arrêt sur image des étapes_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. Joindre une __Pipeline de déploiement hors production__ vers le nouveau référentiel Git
   1. Sélectionner __Pipelines__ dans la navigation supérieure ;
   1. Sélectionner __Ajouter un pipeline__ à partir de la barre d’actions supérieure
   1. Sur le __Configuration__ tab
      1. Sélectionner __Pipeline de déploiement__ option
      1. Définissez la variable __Nom du pipeline hors production__ to `Dev Deployment pipeline`
      1. Sélectionner __Déclencheur de déploiement > Lors des modifications Git__
      1. Sélectionner __Comportement Échecs de mesure importants > Continuer immédiatement__
      1. Sélectionnez __Continuer__
   1. Sur le __Code source__ tab
      1. Sélectionner __Code de pile complet__ option
      1. Sélectionnez la __AEM environnement de développement as a Cloud Service__ de la __Environnements de déploiement éligibles__ zone de sélection
      1. Sélectionner `aem-headless-quick-setup-wknd` dans le __Référentiel__ zone de sélection
      1. Sélectionner `main` de la __Branche Git__ zone de sélection
      1. Sélectionnez __Enregistrer__
1. Exécutez la variable __Pipeline de déploiement de développement__
   1. Sélectionner __Présentation__ dans la navigation supérieure ;
   1. Localisez le __Pipeline de déploiement de développement__ dans le __Pipelines__ section
   1. Sélectionnez la __...__ à droite de l’entrée de pipeline
   1. Sélectionner __Exécuter__ et confirmer dans le modal.
   1. Sélectionnez la __...__ à droite du pipeline en cours d’exécution
   1. Sélectionner __Afficher les détails__
1. Dans les détails de l’exécution du pipeline, surveillez la progression jusqu’à ce qu’elle soit terminée. L’exécution du pipeline doit prendre entre 30 et 40 minutes.

## 4. Téléchargez et exécutez l’application WKND React

Une fois AEM bootstrap as a Cloud Service avec le contenu du projet de site WKND, téléchargez et démarrez l’exemple d’application WKND React qui consomme le contenu du site WKND via les API GraphQL sans affichage AEM.

_Arrêt sur image des étapes_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. À partir de la ligne de commande, clonez le code source de l’application React à partir de GitHub.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Ouvrir le dossier `~/Code/aem-guides-wknd-graphql/react-app` dans votre IDE.
1. Dans l’IDE, ouvrez le fichier . `.env.development`.
1. Pointez sur l’AEM as a Cloud Service __Publier__ URI d’hôte du service à partir de la propriété  `REACT_APP_HOST_URI` .

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   Pour trouver l’URI d’hôte de votre service de publication as a Cloud Service AEM :

   1. Dans Cloud Manager, sélectionnez __Environnements__ dans la navigation supérieure ;
   1. Sélectionner __Développement__ environnement
   1. Recherchez la variable __Service de publication (AEM et Dispatcher)__ link __Segments d’environnement__ table
   1. Copiez l’adresse du lien et utilisez-la comme URI du service de publication as a Cloud Service AEM

1. Dans l’IDE, enregistrez les modifications dans `.env.development`
1. À partir de la ligne de commande, exécutez l’application React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. L’application React, s’exécutant localement, démarre le [http://localhost:3000](http://localhost:3000) et affiche une liste des aventures, issues d’AEM as a Cloud Service à l’aide des API GraphQL d’AEM sans affichage.

## 5. Modifier le contenu dans AEM

Avec l’exemple d’application WKND React se connectant au contenu des API GraphQL sans affichage et l’utilisant, créez du contenu dans le service AEM Author et découvrez comment l’expérience de l’application React se met à jour de concert.

_Arrêt sur image des étapes_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. Connectez-vous au service Auteur as a Cloud Service AEM
1. Accédez à __Ressources > Fichiers > WKND Partagé > Anglais > Aventures__
1. Ouvrez le __Vélo dans l&#39;Utah du Sud__ Dossier
1. Sélectionnez la __Vélo dans l&#39;Utah du Sud__ Fragment de contenu, puis sélectionnez __Modifier__ à partir de la barre d’actions supérieure
1. Mettez à jour certains champs du fragment de contenu, par exemple :
   + Titre : `Cycling Utah's National Parks`
   + Durée du voyage : `6 Days`
   + Difficulté : `Intermediate`
   + Prix: `3500`
   + Image Principal : `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. Sélectionner __Enregistrer__ dans la barre d’actions supérieure
1. Sélectionner __Publication rapide__ dans la barre d’actions supérieure __...__
1. Actualisation de l’application React en cours d’exécution sur [http://localhost:3000](http://localhost:3000).
1. Dans l’application React, sélectionnez l’aventure Cycling mise à jour, puis vérifiez les modifications apportées au contenu du fragment de contenu.

1. En utilisant la même approche, dans le service AEM Author :
   1. Annuler la publication d’un fragment de contenu d’aventure existant et vérifier qu’il est supprimé de l’expérience React App.
   1. Créez et publiez un fragment de contenu Adventure, puis vérifiez qu’il apparaît dans l’expérience React App.

   >[!TIP]
   >
   > Si vous ne connaissez pas la création et la publication de fragments de contenu ou l’annulation de leur publication, regardez la capture d’écran ci-dessus.

## Félicitations !

Félicitations ! Vous avez réussi à utiliser AEM sans affichage pour alimenter une application React !

Pour comprendre en détail comment l’application React consomme du contenu d’AEM as a Cloud Service, extrayez le [Tutoriel AEM sans affichage](../multi-step/overview.md). Le tutoriel explique comment les fragments de contenu d’AEM ont été créés et comment cette application React utilise leur contenu au format JSON.

### Étapes suivantes

+ [Démarrez le tutoriel AEM sans affichage](../multi-step/overview.md)
