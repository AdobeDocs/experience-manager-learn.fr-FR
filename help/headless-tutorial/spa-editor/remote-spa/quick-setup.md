---
title: Configuration rapide SPA Éditeur et SPA à distance
description: Découvrez comment démarrer avec une SPA distante et AEM Éditeur de l'SPA dans 15 minutes !
topic: Sans tête, SPA, développement
feature: Éditeur SPA, composants principaux, API, développement
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
source-git-commit: 6981a1ad5019fd383b1ca1f6fbfbec87e81a6e31
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 6%

---


# Configuration rapide

La configuration rapide est une présentation accélérée qui illustre comment installer et exécuter l’application WKND et en tant que SPA distant, et la créer à l’aide d’AEM Éditeur de ressources.

La configuration rapide vous emmène directement à l’état final de ce tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/333181/?quality=12&learn=on)

_Présentation vidéo de la configuration rapide_

## Prérequis

Ce tutoriel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Conditions préalables uniquement pour macOS
   + [](https://developer.apple.com/xcode/) Outils de ligne de commande Xcode ou  [Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all.0.3.0.zip ou version ultérieure](https://github.com/adobe/aem-guides-wknd/releases)
+ [code source aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)


Ce tutoriel suppose :

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas de l’IDE
+ Un répertoire de travail de `~/Code/wknd-app`
+ Exécution du SDK AEM en tant que service Auteur sur `http://localhost:4502`
+ Exécution du SDK AEM avec le compte `admin` local avec le mot de passe `admin`
+ Exécution du SPA sur `http://localhost:3000`

## Démarrage rapide du SDK AEM

Téléchargez et installez le SDK AEM Quickstart sur le port 4502, avec les informations d’identification `admin/admin` par défaut.

1. [Téléchargement du dernier SDK AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Décompressez le SDK AEM sur `~/aem-sdk`
1. Exécution du fichier Jar de démarrage rapide du SDK AEM

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK démarre et se lance automatiquement le [http://localhost:4502](http://localhost:4502). Connectez-vous à l’aide des informations d’identification suivantes :

+ Nom d’utilisateur: `admin`
+ Mot de passe: `admin`

## Télécharger et installer le package WKND Site

Ce tutoriel dépend du projet __WKND 0.3.0+__ (pour le contenu).

1. [Téléchargez la dernière version de  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Connectez-vous au gestionnaire de modules du SDK AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) à l’aide des informations d’identification `admin`.
1. ____ Télécharger le  `aem-guides-wknd.all.x.x.x.zip` téléchargé à l’étape 1
1. Appuyez sur le bouton __Installer__ pour l’entrée `aem-guides-wknd.all-x.x.x.zip`

## Télécharger et installer des packages SPA d’application WKND

Pour effectuer une configuration rapide, AEM modules contenant la configuration et le contenu de l’AEM finale du tutoriel sont fournis.

1. [Télécharger ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Télécharger ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. Connectez-vous au gestionnaire de modules du SDK AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) à l’aide des informations d’identification `admin`.
1. ____ Télécharger le  `wknd-app.all.x.x.x.zip` téléchargé à l’étape 1
1. Appuyez sur le bouton __Installer__ pour l’entrée `wknd-app.all.x.x.x.zip`
1. ____ Télécharger le  `wknd-app.ui.content.sample.x.x.x.zip` téléchargé à l’étape 2
1. Appuyez sur le bouton __Installer__ pour l’entrée `wknd-app.ui.content.sample.x.x.x.zip`

## Téléchargement de la source de l’application WKND

Téléchargez le code source de l’application WKND à partir de Github.com et basculez la branche contenant les modifications vers le SPA effectué dans ce tutoriel.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Démarrez l’application SPA

À partir de la racine du projet, installez les dépendances npm SPA projets et exécutez l’application.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

En cas d&#39;erreur lors de l&#39;exécution de `npm install`, procédez comme suit :

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Vérifiez que le SPA s’exécute à [http://localhost:3000](http://localhost:3000).

## Création de contenu dans AEM Éditeur SPA

Avant de créer du contenu, organisez les fenêtres de votre navigateur de sorte qu’AEM Author (`http://localhost:4502`) soit à gauche et que la SPA distante (`http://localhost:3000`) s’exécute à droite. Cet arrangement vous permet de voir comment les modifications apportées au contenu AEM sont immédiatement répercutées dans le SPA.

1. Connectez-vous à [AEM service d’auteur du SDK](http://localhost:4502) en tant que `admin`
1. Accédez à __Sites > Application WKND > us > en__
1. Modifier __Page d’accueil de l’application WKND__
1. Basculer vers le mode __Modifier__

### Création du composant fixe de la vue d’accueil

1. Appuyez sur le texte __WKND Adventures__ pour activer le composant de titre fixe (codé en dur dans la vue d’accueil SPA).
1. Appuyez sur l’icône __clé à molette__ dans la barre d’actions du composant Titre.
1. Modifie le contenu du composant Titre et enregistre
1. Actualisez le SPA exécuté sur `http://localhost:3000` et vérifiez que les modifications sont répercutées.

### Création du composant Conteneur de la vue d’accueil

1. Lors de la modification de la __page d’accueil de l’application WKND__...
1. Développez la __SPA barre latérale de l’éditeur__ (à gauche).
1. Appuyez sur les icônes __Composants__ .
1. Ajouter, modifier ou supprimer des composants du composant de conteneur situé sous le logo WKND et au-dessus du composant Titre fixe
1. Actualisez le SPA exécuté sur `http://localhost:3000` et vérifiez que les modifications sont répercutées.

### Création d’un composant de conteneur sur un itinéraire dynamique

1. Basculer vers le mode __Aperçu__ dans SPA éditeur
1. Appuyez sur la carte __Bali Surf Camp__ et accédez à son itinéraire dynamique.
1. Ajouter, modifier ou supprimer des composants du composant de conteneur situé au-dessus de l’en-tête __Initiaire__
1. Actualisez le SPA exécuté sur `http://localhost:3000` et vérifiez que les modifications sont répercutées.

Les nouvelles pages d’AEM sous la __Page d’accueil de l’application WKND > Adventure__ _doivent_ avoir un nom de page AEM qui correspond au nom du fragment de contenu de l’aventure correspondante. En effet, l’itinéraire SPA vers AEM mappage de page est basé sur le dernier segment de l’itinéraire, qui est le nom du fragment de contenu.

## Félicitations ! 

Vous venez d’apprendre rapidement comment l’éditeur de SPA d’AEM peut améliorer votre  avec des zones contrôlées et modifiables ! Si vous êtes intéressé, consultez le reste du tutoriel, mais assurez-vous de commencer à nouveau, puisque dans cette configuration rapide, votre environnement de développement local est maintenant à l’état final du tutoriel !
