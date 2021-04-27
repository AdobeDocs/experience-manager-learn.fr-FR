---
title: Configuration rapide SPA éditeur et SPA à distance
description: Apprenez à vous familiariser avec un SPA et un AEM-éditeur à distance en 15 minutes !
topic: Sans tête, SPA, développement
feature: SPA Éditeur, Composants principaux, API, Développement
role: Developer, Architect
level: Beginner
kt: 7629
thumbnail: kt-7629.jpeg
translation-type: tm+mt
source-git-commit: b6f63110f14ede51fa2dd740aea7cbb623cbec60
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 5%

---


# Configuration rapide

La configuration rapide est une procédure accélérée qui illustre comment installer et exécuter l&#39;application WKND et en tant que SPA à distance, et comment la créer à l&#39;aide d&#39;AEM Éditeur.

La configuration rapide vous conduit directement à l’état final de ce didacticiel.

## Conditions préalables

Ce didacticiel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip ou version ultérieure](https://github.com/adobe/aem-guides-wknd/releases)
+ [code source aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)

Ce didacticiel suppose :

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas the IDE
+ Un répertoire de travail de `~/Code/wknd-app`
+ Exécution du SDK AEM en tant que service Auteur sur `http://localhost:4502`
+ Exécution du SDK AEM avec le compte `admin` local avec le mot de passe `admin`
+ Exécution du SPA le `http://localhost:3000`

## Début du démarrage rapide du SDK AEM

Téléchargez et installez l’AEM SDK Quickstart sur le port 4502, avec les informations d’identification `admin/admin` par défaut.

1. [Télécharger le dernier SDK AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Décompressez le SDK AEM vers `~/aem-sdk`.
1. Exécution du JAR de démarrage rapide du SDK AEM

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

AEM SDK début et se lance automatiquement le [http://localhost:4502](http://localhost:4502). Connectez-vous à l’aide des informations d’identification suivantes :

+ Nom d’utilisateur: `admin`
+ Mot de passe: `admin`

## Téléchargement et installation du package du site WKND

Ce didacticiel dépend du projet __WKND 0.3.0+__ (pour le contenu).

1. [Téléchargez la dernière version de  `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Connectez-vous à Package Manager du SDK AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) avec les informations d’identification `admin`.
1. ____ Télécharger le  `aem-guides-wknd.all.x.x.x.zip` téléchargé à l’étape 1
1. Appuyez sur le bouton __Installer__ pour l&#39;entrée `aem-guides-wknd.all-x.x.x.zip`

## Téléchargement et installation des packages de SPA d’applications WKND

Pour effectuer une configuration rapide, AEM packages sont fournis qui contiennent la configuration et le contenu AEM final du didacticiel.

1. [Télécharger `wknd-app.all.x.x.x.zip`](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Télécharger `wknd-app.ui.content.sample.x.x.x.zip`](./assets/quick-setup/wknd-app.ui.content.sample-1.0.0.zip)
1. Connectez-vous à Package Manager du SDK AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) avec les informations d’identification `admin`.
1. ____ Télécharger le  `wknd-app.all.x.x.x.zip` téléchargé à l’étape 1
1. Appuyez sur le bouton __Installer__ pour l&#39;entrée `wknd-app.all.x.x.x.zip`
1. ____ Télécharger le  `wknd-app.ui.content.sample.x.x.x.zip` téléchargé à l’étape 2
1. Appuyez sur le bouton __Installer__ pour l&#39;entrée `wknd-app.ui.content.sample.x.x.x.zip`

## Téléchargement de la source de l’application WKND

Téléchargez le code source de l’application WKND depuis Github.com, puis changez la branche contenant les modifications apportées au SPA effectué dans ce didacticiel.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
$ git checkout -b feature/spa-editor
```

## Début de l’application SPA

A partir de la racine du projet, installez les dépendances npm des projets SPA et exécutez l&#39;application.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

Si des erreurs se produisent lors de l&#39;exécution de `npm install`, procédez comme suit :

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Vérifiez que le SPA s’exécute à l’adresse [http://localhost:3000](http://localhost:3000).

## Création de contenu dans AEM SPA Editor

Avant de créer du contenu, disposez les fenêtres de votre navigateur de telle sorte qu’AEM Author (`http://localhost:4502`) se trouve à gauche et que la SPA distante (`http://localhost:3000`) s’exécute à droite. Cet arrangement vous permet de voir comment les modifications apportées au contenu d’AEM source sont immédiatement répercutées dans le SPA.

1. Connectez-vous au [service d’auteur AEM SDK](http://localhost:4502) en tant que `admin`
1. Accédez à __Sites > Application WKND > us > en__
1. Modifier __Page d&#39;accueil d’application WKND__
1. Basculer vers le mode __Modifier__

### Création du composant fixe de la vue d’accueil

1. Appuyez sur le texte __WKND Adventures__ pour activer le composant de titre fixe (codé en dur dans la vue d’accueil SPA).
1. Appuyez sur l&#39;icône __clé à molette__ de la barre d&#39;action du composant Titre.
1. Modifie le contenu du composant Titre et enregistre
1. Actualisez le SPA en cours d’exécution sur `http://localhost:3000` et vérifiez que les modifications reflètent

### Création du composant conteneur de la vue d’accueil

1. Lors de la modification de la __Page d&#39;accueil d’application WKND__...
1. Développez la barre latérale __SPA Editeur__ (à gauche).
1. Appuyez sur les icônes __Composants__.
1. Ajouter, modifier ou supprimer des composants du composant de conteneur situés sous le logo WKND et au-dessus du composant de titre fixe
1. Actualisez le SPA en cours d’exécution sur `http://localhost:3000` et vérifiez que les modifications reflètent

### Création d’un composant de conteneur sur une route dynamique

1. Basculer vers le mode __Prévisualisation__ dans SPA Éditeur
1. Appuyez sur la carte __Bali Surf Camp__ et naviguez jusqu’à son itinéraire dynamique.
1. Ajouter, modifier ou supprimer des composants du composant de conteneur qui se trouvent au-dessus de l&#39;en-tête __Itinéraire__
1. Actualisez le SPA en cours d’exécution sur `http://localhost:3000` et vérifiez que les modifications reflètent

## Félicitations ! 

Vous venez juste de découvrir comment AEM SPA Editor peut améliorer votre SPA avec des zones contrôlées et modifiables ! Si vous êtes intéressés - regardez le reste du tutoriel, mais assurez-vous de le début frais, puisque dans cette configuration rapide votre environnement de développement local est maintenant en état de fin du tutoriel !
