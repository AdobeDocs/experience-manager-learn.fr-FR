---
title: Configuration des outils de développement pour AEM en tant que Cloud Service de développement
description: Configurez une machine de développement locale avec tous les outils de base nécessaires pour se développer par rapport aux AEM locaux.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: cb5f3c323c433c9321ba26ac1194be0cd225a405
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 2%

---


# Configuration des outils de développement

Le développement de Adobe Experience Manager (AEM) nécessite un ensemble minimal d&#39;outils de développement à installer et à configurer sur la machine de développement. Ces outils soutiennent le développement et la création de projets AEM.

Notez que `~` est utilisé comme abrégé pour le Répertoire d&#39;utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

## Installer Java

Experience Manager est une application Java et nécessite donc le SDK Java pour prendre en charge le développement et l’AEM en tant que SDK Cloud Service.

1. [Téléchargement et installation de la dernière version du SDK Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent 2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=liste&amp;p.offset=0&amp;p.limit=14)
1. Vérifiez que le SDK Java 11 est installé en exécutant la commande :
   + Windows : `java -version`
   + macOS / Linux : `java --version`

![Java](./assets/development-tools/java.png)

## Installer Homebrew

_L&#39;utilisation de l&#39;hébreu est facultative, mais recommandée._

Homebrew est un gestionnaire de paquets open-source pour macOS, Windows et Linux. Tous les outils de support peuvent être installés séparément, Homebrew offre un moyen pratique d&#39;installer et de mettre à jour une variété d&#39;outils de développement requis pour le développement Experience Manager.

1. Ouvrir votre terminal
1. Vérifiez si Homebrew est déjà installé en exécutant la commande : `brew --version`.
1. Si Homebrew n’est pas installé, installez Homebrew.
   + [Installer Homebrew sur macOS](https://brew.sh/)
      + Homebrew sur macOS requiert [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou [Outils de ligne de commande](https://developer.apple.com/download/more/), installable via la commande :
         + `xcode-select --install`
   + [Installer Homebrew sous Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Installer Homebrew sous Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Vérifiez que Homebrew est installé en exécutant la commande : `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Si vous utilisez Homebrew, suivez les instructions __Installer à l&#39;aide de Homebrew__ dans les sections ci-dessous. Si __vous n’utilisez pas__ Homebrew, installez les outils à l’aide des liens propres au système d’exploitation.

## Installer Git

[](https://git-scm.com/) Gitis est le système de gestion du contrôle de code source utilisé par  [Adobe Cloud Manager](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html) et est donc requis pour le développement.

+ Installer Git à l&#39;aide de Homebrew
   1. Ouvrez l&#39;invite de commande/terminal.
   1. Exécutez la commande : `brew install git`
   1. Vérifiez que Git est installé à l’aide de la commande : `git --version`
+ Ou, téléchargez et installez Git (macOS, Linux ou Windows).
   1. [Téléchargement et installation de Git](https://git-scm.com/downloads)
   1. Ouvrez l&#39;invite de commande/terminal.
   1. Vérifiez que Git est installé à l’aide de la commande : `git --version`

![Git](./assets/development-tools/git.png)

## Installer Node.js (et npm){#node-js}

[Node.](https://nodejs.org) jsis est un environnement d’exécution JavaScript utilisé pour travailler avec les actifs frontaux du  __projet ui.__ frontendsub d’un projet AEM. Node.js est distribué avec [npm](https://www.npmjs.com/), est le gestionnaire de package de Node.js utilisé par défaut pour gérer les dépendances JavaScript.

+ Installation de Node.js à l’aide de Homebrew
   1. Ouvrez l&#39;invite de commande/terminal.
   1. Exécutez la commande : `brew install node`
   1. Vérifiez que Node.js est installé à l’aide de la commande : `node -v`
   1. Vérifiez que npm est installé à l&#39;aide de la commande : `npm -v`
+ Ou, téléchargez et installez Node.js (macOS, Linux ou Windows).
   1. [Téléchargement et installation de Node.js](https://nodejs.org/en/download/)
   1. Ouvrez l&#39;invite de commande/terminal.
   1. Vérifiez que Node.js est installé à l’aide de la commande : `node -v`
   1. Vérifiez que npm est installé à l&#39;aide de la commande : `npm -v`

![Node.js et npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[Les AEM Projets basés sur l’archétype](https://github.com/adobe/aem-project-archetype) de projets AEM installent une version isolée de Node.js au moment de la création. Il est bon de conserver la version du système de développement local synchronisée (ou proche) des versions de Node.js et npm spécifiées dans le fichier Reactor pom.xml de votre projet AEM Maven.
>
>Consultez cet exemple [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) pour connaître l’emplacement des versions de création Node.js et npm.

## Installation de Maven

Apache Maven est l&#39;outil de ligne de commande Java open-source utilisé pour construire AEM projets générés à partir de l&#39;archétype AEM Project Maven. Tous les principaux IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Code Visual Studio](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) sont pris en charge par Maven.

+ Installation de Maven à l’aide de Homebrew
   1. Ouvrez l&#39;invite de commande/terminal.
   1. Exécutez la commande : `brew install maven`
   1. Vérifiez que Maven est installé à l’aide de la commande : `mvn -v`
+ Ou, téléchargez et installez Maven (macOS, Linux ou Windows)
   1. [Télécharger Maven](https://maven.apache.org/download.cgi)
   1. [Installation de Maven](https://maven.apache.org/install.html)
   1. Ouvrez l&#39;invite de commande/terminal.
   1. Vérifiez que Maven est installé à l’aide de la commande : `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configuration de l’interface de ligne de commande Adobe I/O{#aio-cli}

L&#39;interface de ligne de commande [Adobe I/O ](https://github.com/adobe/aio-cli), ou `aio`, permet d&#39;accéder en ligne de commande à divers services d&#39;Adobe, y compris [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) et [Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). L&#39;interface de ligne de commande Adobe I/O joue un rôle essentiel dans le développement de l&#39;AEM en tant que Cloud Service car elle permet aux développeurs de :

+ Journaux de suivi des AEM en tant que services Cloud Services
+ Gestion des pipelines Cloud Manager à partir de l’interface de ligne de commande

### Installation de l’interface de ligne de commande Adobe I/O

1. Assurez-vous que [Node.js est installé](#node-js) car l’interface de ligne de commande Adobe I/O est un module npm.
   + Exécutez `node --version` pour confirmer
1. Exécutez `npm install -g @adobe/aio-cli` pour installer le module `aio` npm globalement.

### Configuration du module externe Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

Le module externe Adobe I/O Cloud Manager permet à l’interface de ligne de commande d’AEM d’interagir avec Adobe Cloud Manager via la commande `aio cloudmanager`.

1. Exécutez `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` pour installer le module externe [aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Configuration du module externe d’Asset compute de l’interface de ligne de commande Adobe I/O{#aio-asset-compute}

Le module externe Adobe I/O Cloud Manager permet à l’interface de ligne de commande d’aio de générer et d’exécuter des agents d’Asset compute via la commande `aio asset-compute`.

1. Exécutez `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` pour installer le module externe [aio Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configuration de l’authentification CLI Adobe I/O

Pour que l’interface de ligne de commande Adobe I/O communique avec Cloud Manager, une intégration de Cloud Manager doit être créée dans Adobe I/O Console et des informations d’identification doivent être obtenues pour une authentification réussie.

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. Connectez-vous à [console.adobe.io](https://console.adobe.io)
1. Assurez-vous que votre organisation qui comprend le produit Cloud Manager auquel se connecter est principale dans le sélecteur d’organisation d’Adobes.
1. Créez ou ouvrez un [programme Adobe I/O ](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) existant.
   + Les programmes de la Console Adobe I/O sont simplement des regroupements organisationnels d&#39;intégrations, de créer ou d&#39;utiliser et de programmes existants basés sur la façon dont vous souhaitez gérer vos intégrations
   + Si vous créez un projet, sélectionnez &quot;Projet vide&quot; si vous y êtes invité (par rapport à Créer à partir d’un modèle).
   + Les programmes de la console Adobe I/O sont différents concepts pour les programmes de Cloud Manager.
1. Créer une nouvelle intégration de l’API Cloud Manager avec le profil &quot;Développeur - Cloud Service&quot;
1. Obtenez les informations d’identification du compte de service (JWT) nécessaires pour renseigner l’interface de ligne de commande Adobe I/O [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication).
1. Charger le fichier `config.json` dans l’interface de ligne de commande Adobe I/O
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. Charger le fichier `private.key` dans l’interface de ligne de commande Adobe I/O
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

Commencez [à exécuter des commandes](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) pour Cloud Manager via l’interface de ligne de commande Adobe I/O.

## Configuration de l&#39;IDE de développement

Le développement AEM consiste principalement en le développement Java et frontal (JavaScript, CSS, etc.) et la gestion XML. Voici les IDE les plus utilisés pour le développement AEM.

### IDÉE IntelliJ

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEAest un puissant IDE pour le développement de Java. IntelliJ IDEA est disponible en deux versions, une édition communautaire gratuite et une version finale commerciale (payante). La version communautaire gratuite est suffisante pour le développement AEM, mais l&#39;Ultimate [étend sa capacité ](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Télécharger IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Téléchargement de l&#39;outil Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Code Microsoft Visual Studio

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code) est un outil gratuit et open source pour les développeurs frontaux. Le code Visual Studio peut être configuré pour intégrer la synchronisation de contenu avec AEM à l&#39;aide d&#39;un outil d&#39;Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code est le choix idéal pour les développeurs frontaux qui créent principalement du code frontal ; JavaScript, CSS et HTML. Bien que le code VS dispose de la prise en charge de Java via [extensions](https://code.visualstudio.com/docs/java/java-tutorial), il peut ne pas disposer de certaines fonctionnalités avancées fournies par des fonctionnalités plus spécifiques à Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Téléchargement du code Visual Studio](https://code.visualstudio.com/Download)
+ [Téléchargement de l&#39;outil Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Téléchargement de l&#39;extension de code VS aemfed](https://aemfed.io/)
+ [Télécharger AEM Sync VS Code extension](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDEs est un IDE très utilisé pour le développement de Java. Il prend en charge le module externe   __[AEM Developer ](https://eclipse.adobe.com/aem/dev-tools/)__ Toolsfourni par Adobe, fournissant une interface utilisateur graphique intégrée pour la création et la synchronisation de contenu JCR avec une instance AEM locale.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Télécharger Eclipse](https://www.eclipse.org/ide/)
+ [Téléchargement des outils de développement Eclipse](https://eclipse.adobe.com/aem/dev-tools/)
