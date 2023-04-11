---
title: Configuration rapide SPA Éditeur et SPA à distance
description: Découvrez comment démarrer avec une SPA distante et AEM Éditeur de l'SPA dans 15 minutes !
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: 333181.jpg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: ef7a1dad-993a-4c47-a9fb-91fa73de9b5d
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 5%

---

# Configuration rapide

La configuration rapide est une présentation accélérée qui illustre comment installer et exécuter l’application WKND et en tant que SPA distant, et la créer à l’aide d’AEM Éditeur de ressources.

La configuration rapide vous emmène directement à l’état final de ce tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Présentation vidéo de la configuration rapide_

## Prérequis

Ce tutoriel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Conditions préalables pour macOS uniquement
   + [Xcode](https://developer.apple.com/xcode/) ou [Outils de ligne de commande Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip ou version ultérieure](https://github.com/adobe/aem-guides-wknd/releases)
+ [code source aem-guides-wknd-graphql (branche) : feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Ce tutoriel suppose :

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) comme IDE
+ Un répertoire de travail de `~/Code/wknd-app`
+ Exécution du SDK AEM en tant que service d’auteur sur `http://localhost:4502`
+ Exécution du SDK AEM avec le fichier local `admin` compte avec mot de passe `admin`
+ Exécution du SPA sur `http://localhost:3000`

## Démarrage rapide du SDK AEM

Téléchargez et installez le SDK AEM Quickstart sur le port 4502, avec la valeur par défaut. `admin/admin` informations d’identification.

1. [Téléchargement du dernier SDK AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Décompressez le SDK AEM pour `~/aem-sdk`
1. Exécution du fichier Jar de démarrage rapide du SDK AEM

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK démarre et se lance automatiquement sur [http://localhost:4502](http://localhost:4502). Connectez-vous à l’aide des informations d’identification suivantes :

+ Nom d’utilisateur: `admin`
+ Password: `admin`

## Télécharger et installer le package WKND Site

Ce tutoriel dépend de __WKND 2.1.0+__ projet (pour le contenu).

1. [Téléchargez la dernière version de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Connectez-vous au gestionnaire de modules du SDK AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) avec le `admin` informations d’identification.
1. __Télécharger__ la valeur `aem-guides-wknd.all.x.x.x.zip` téléchargé à l’étape 1
1. Appuyez sur le bouton __Installer__ pour l’entrée `aem-guides-wknd.all-x.x.x.zip`

## Télécharger et installer des packages SPA d’application WKND

Pour effectuer une configuration rapide, AEM modules qui contiennent la configuration et le contenu finaux AEM du tutoriel sont fournis ici.

1. [Télécharger ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Télécharger ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Connectez-vous au gestionnaire de modules du SDK AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) avec le `admin` informations d’identification.
1. __Télécharger__ la valeur `wknd-app.all.x.x.x.zip` téléchargé à l’étape 1
1. Appuyez sur le bouton __Installer__ pour l’entrée `wknd-app.all.x.x.x.zip`
1. __Télécharger__ la valeur `wknd-app.ui.content.sample.x.x.x.zip` téléchargé à l’étape 2
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

En cas d’erreur lors de l’exécution `npm install` essayez les étapes suivantes :

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Vérifiez que le SPA est en cours d’exécution à l’adresse [http://localhost:3000](http://localhost:3000).

## Création de contenu dans AEM Éditeur SPA

Avant de créer du contenu, organisez les fenêtres de votre navigateur de sorte qu’AEM Author (`http://localhost:4502`) se trouve à gauche et la SPA distante (`http://localhost:3000`) s’exécute à droite. Cet arrangement vous permet de voir comment les modifications apportées au contenu AEM sont immédiatement répercutées dans le SPA.

1. Connectez-vous à [Service d’auteur AEM SDK](http://localhost:4502) as `admin`
1. Accédez à __Sites > Application WKND > us > en__
1. Modifier __Page d’accueil de l’application WKND__
1. Basculer vers __Modifier__ mode

### Création du composant fixe de la vue d’accueil

1. Appuyez sur le texte. __Les aventures de WKND__ pour activer le composant Titre fixe (codé en dur dans la vue d’accueil SPA).
1. Appuyez sur le bouton __clé à molette__ icône dans la barre d’actions du composant Titre
1. Modifie le contenu du composant Titre et enregistre
1. Actualisez le SPA en cours d’exécution `http://localhost:3000` et vérifier que les modifications sont reflétées

### Création du composant Conteneur de la vue d’accueil

1. Lors de la modification de la variable __Page d’accueil de l’application WKND__...
1. Développez l’objet __SPA barre latérale de l’éditeur__ (à gauche)
1. Appuyez sur le bouton __Composants__ icônes
1. Ajouter, modifier ou supprimer des composants du composant de conteneur situé sous le logo WKND et au-dessus du composant Titre fixe
1. Actualisez le SPA en cours d’exécution `http://localhost:3000` et vérifier que les modifications sont reflétées

### Création d’un composant de conteneur sur un itinéraire dynamique

1. Basculer vers __Aperçu__ Mode dans SPA Editor
1. Appuyez sur le __Le camp de surf de Bali__ carte et accéder à son itinéraire dynamique
1. Ajoutez, modifiez ou supprimez des composants du composant de conteneur situé au-dessus du __Itinéraire__ titre
1. Actualisez le SPA en cours d’exécution `http://localhost:3000` et vérifier que les modifications sont reflétées

Nouvelles pages d’AEM sous __Page d’accueil de l’application WKND > Aventure__ _must_ avoir un nom de page d’AEM correspondant au nom du fragment de contenu de l’aventure correspondante. En effet, l’itinéraire SPA vers AEM mappage de page est basé sur le dernier segment de l’itinéraire, qui est le nom du fragment de contenu.

## Félicitations !

Vous venez d’apprendre rapidement comment l’éditeur de SPA d’AEM peut améliorer votre  avec des zones contrôlées et modifiables ! Si vous êtes intéressé, consultez le reste du tutoriel, mais assurez-vous de commencer à nouveau, puisque dans cette configuration rapide, votre environnement de développement local est maintenant à l’état final du tutoriel !
