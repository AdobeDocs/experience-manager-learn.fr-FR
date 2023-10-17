---
title: Configuration rapide de l’éditeur de SPA et de la SPA distante
description: Découvrez comment être à pied d’œuvre avec une SPA distante et l’éditeur de SPA d’AEM en 15 minutes.
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
workflow-type: ht
source-wordcount: '793'
ht-degree: 100%

---

# Configuration rapide

La configuration rapide est un parcours accéléré qui illustre comment installer et exécuter l’application WKND en tant que SPA distante, puis la créer à l’aide de l’éditeur de SPA d’AEM.

La configuration rapide vous emmène directement au résultat final de ce tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/333181?quality=12&learn=on)

_Présentation vidéo de la configuration rapide._

## Prérequis

Ce tutoriel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr)
+ [Node.js v18](https://nodejs.org/fr/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6 et ultérieure](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ Conditions préalables pour macOS uniquement
   + [Xcode](https://developer.apple.com/xcode/) ou [outils de ligne de commande Xcode](https://developer.apple.com/xcode/resources/)
+ [aem-guides-wknd.all-2.1.0.zip ou version ultérieure](https://github.com/adobe/aem-guides-wknd/releases)
+ [Code source aem-guides-wknd-graphql (branche : feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)


Ce tutoriel fonctionne avec les éléments suivants :

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) comme IDE
+ Un répertoire de travail de `~/Code/wknd-app`
+ Exécution du SDK d’AEM en tant que service de création sur `http://localhost:4502`
+ Exécution du SDKd’ AEM avec le compte `admin` local et le mot de passe `admin`
+ Exécution de la SPA sur `http://localhost:3000`

## Lancer le démarrage rapide du SDK d’AEM

Téléchargez et installez le démarrage rapide du SDK d’AEM sur le port 4502, avec les informations d’identification `admin/admin` par défaut.

1. [Télécharger le dernier SDK d’AEM](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)
1. Décompressez le SDK d’AEM vers `~/aem-sdk`.
1. Exécutez le fichier Jar de démarrage rapide du SDK d’AEM.

   ```
   $ java -jar aem-sdk-quickstart-xxx.jar
   
   # Provide `admin` as the admin user's password
   ```

Le SDK d’AEM démarre et se lance automatiquement sur [http://localhost:4502](http://localhost:4502). Connectez-vous à l’aide des informations d’identification suivantes :

+ Nom d’utilisateur : `admin`.
+ Mot de passe : `admin`.

## Télécharger et installer le package WKND Site

Ce tutoriel a une dépendance au projet __WKND 2.1.0+__ (pour le contenu).

1. [Télécharger la dernière version de `aem-guides-wknd.all.x.x.x.zip`](https://github.com/adobe/aem-guides-wknd/releases)
1. Connectez-vous au gestionnaire de packages du SDK d’AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) avec les informations d’identification `admin`.
1. __Chargez__ le fichier `aem-guides-wknd.all.x.x.x.zip` téléchargé à l’étape 1.
1. Appuyez sur le bouton __Installer__ pour l’entrée `aem-guides-wknd.all-x.x.x.zip`.

## Télécharger et installer des packages SPA d’application WKND

Pour effectuer une configuration rapide, des packages AEM qui contiennent la configuration et le contenu finaux du tutoriel d’AEM sont fournis ici.

1. [Télécharger ](./assets/quick-setup/wknd-app.all-1.0.0-SNAPSHOT.zip)
1. [Télécharger ](./assets/quick-setup/wknd-app.ui.content.sample-1.0.1.zip)
1. Connectez-vous au gestionnaire de packages du SDK d’AEM à l’adresse [http://localhost:4502/crx/packmgr](http://localhost:4502/crx/packmgr) avec les informations d’identification `admin`.
1. __Chargez__ le fichier `wknd-app.all.x.x.x.zip` téléchargé à l’étape 1.
1. Appuyez sur le bouton __Installer__ pour l’entrée `wknd-app.all.x.x.x.zip`.
1. __Chargez__ le fichier `wknd-app.ui.content.sample.x.x.x.zip` téléchargé à l’étape 2.
1. Appuyez sur le bouton __Installer__ pour l’entrée `wknd-app.ui.content.sample.x.x.x.zip`.

## Télécharger la source de l’application WKND

Téléchargez le code source de l’application WKND à partir de Github.com et basculez la branche contenant les modifications vers la SPA réalisée dans ce tutoriel.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Démarrer l’application SPA

À partir de la racine du projet, installez les dépendances npm des projets SPA et exécutez l’application.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install
$ npm run start
```

En cas d’erreur lors de l’exécution de `npm install`, essayez les étapes suivantes :

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ rm -f package-lock.json
$ npm install --legacy-peer-deps
$ npm run start
```

Vérifiez que la SPA est en cours d’exécution à l’adresse [http://localhost:3000](http://localhost:3000).

## Créer du contenu dans l’éditeur de SPA d’AEM

Avant de créer du contenu, organisez les fenêtres du navigateur de sorte que l’instance de création AEM (`http://localhost:4502`) se trouve à gauche et que la SPA distante (`http://localhost:3000`) s’exécute à droite. Cet arrangement vous permet de voir comment les modifications apportées au contenu provenant d’AEM sont immédiatement répercutées dans la SPA.

1. Connectez-vous au [service de création du SDK d’AEM](http://localhost:4502) en tant que `admin`.
1. Accédez à __Sites > Application WKND > us > en__.
1. Modifiez la __Page d’accueil de l’application WKND__.
1. Basculez vers le mode __Modifier__.

### Créer le composant fixe de la vue d’accueil

1. Appuyez sur le texte __WKND Adventures__ pour activer le composant fixe de titre (codé en dur dans la vue d’accueil de la SPA).
1. Appuyez sur l’icône __clé à molette__ dans la barre d’actions du composant de titre.
1. Modifiez le contenu du composant de titre et enregistrez.
1. Actualisez la SPA exécutée sur `http://localhost:3000` et vérifiez que les modifications ont été prises en compte.

### Créer le composant de conteneur de la vue d’accueil

1. Lors de la modification de la __Page d’accueil de l’application WKND__ :
1. Développez la __barre latérale de l’éditeur de SPA__ (à gauche).
1. Appuyez sur les icônes des __Composants__.
1. Ajoutez, modifiez ou supprimez des composants du composant de conteneur situé sous le logo WKND et au-dessus du composant fixe de titre.
1. Actualisez la SPA exécutée sur `http://localhost:3000` et vérifiez que les modifications ont été prises en compte.

### Créer un composant de conteneur sur un itinéraire dynamique

1. Basculez vers le mode __Aperçu__ dans l’éditeur de SPA.
1. Appuyez sur la carte de __Bali Surf Camp__ et accédez à l’itinéraire dynamique.
1. Ajoutez, modifiez ou supprimez des composants du composant de conteneur situé au-dessus de l’en-tête __Itinerary__.
1. Actualisez la SPA exécutée sur `http://localhost:3000` et vérifiez que les modifications ont été prises compte.

Les nouvelles pages d’AEM dans __Page d’accueil de l’application WKND > Adventure__ _doivent_ avoir un nom de page AEM correspondant au nom du fragment de contenu de l’Adventure associée. En effet, l’itinéraire de la SPA vers le mappage de page d’AEM est basé sur le dernier segment de l’itinéraire, c’est-à-dire le nom du fragment de contenu.

## Félicitations.

Vous venez de voir rapidement comment l’éditeur de SPA d’AEM peut améliorer votre SPA grâce à des domaines contrôlés et modifiables. Si cela vous intéresse, consultez le reste du tutoriel. Assurez-vous de repartir de zéro, car dans cette configuration rapide, votre environnement de développement local se trouve désormais à l’état final du tutoriel.
