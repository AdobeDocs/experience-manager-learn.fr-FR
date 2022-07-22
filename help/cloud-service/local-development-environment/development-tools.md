---
title: Configuration des outils de développement pour AEM développement as a Cloud Service
description: Configurez une machine de développement locale avec tous les outils de base nécessaires pour le développement local par rapport à AEM.
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 3%

---

# Configuration des outils de développement {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configuration des outils de développement"
>abstract="Le développement d’Adobe Experience Manager (AEM) nécessite l’installation et la configuration d’un ensemble minimal d’outils de développement sur l’ordinateur du développeur. Ces outils comprennent Java, Maven, Adobe I/O CLI, Development IDE, etc."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=fr" text="Conseils de développement"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Principes de développement"

Le développement d’Adobe Experience Manager (AEM) nécessite l’installation et la configuration d’un ensemble minimal d’outils de développement sur l’ordinateur du développeur. Ces outils prennent en charge le développement et la création de projets AEM.

Notez que `~` est utilisé comme abrégé pour le répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

## Installer Java

Experience Manager est une application Java qui requiert donc le SDK Java pour prendre en charge le développement et le SDK as a Cloud Service AEM.

1. [Téléchargez et installez la dernière version du SDK Java 11.](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Vérifiez que le SDK Java 11 est installé en exécutant la commande :
   + Windows : `java -version`
   + macOS / Linux : `java --version`

![Java](./assets/development-tools/java.png)

## Installer Homebrew

_L’utilisation de la saumure est facultative, mais recommandée._

Homebrew est un gestionnaire de packages Open Source pour macOS, Windows et Linux. Tous les outils de prise en charge peuvent être installés séparément. Homebrew offre un moyen pratique d’installer et de mettre à jour divers outils de développement requis pour le développement de Experience Manager.

1. Ouvrez votre terminal.
1. Vérifiez si Homebrew est déjà installé en exécutant la commande : `brew --version`.
1. Si Homebrew n’est pas installé, installez Homebrew.
   + [Installation de Homebrew sur macOS](https://brew.sh/)
      + Le raccourci sur macOS nécessite [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou [Outils de ligne de commande](https://developer.apple.com/download/more/), install-able via la commande :
         + `xcode-select --install`
   + [Installer Homebrew sous Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [Installation de Homebrew sous Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. Vérifiez que Homebrew est installé en exécutant la commande : `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

Si vous utilisez Homebrew, suivez la __Installation à l’aide de Homebrew__ dans les sections ci-dessous. Si vous êtes __not__ à l’aide d’Homebrew, installez les outils à l’aide des liens spécifiques au système d’exploitation.

## Installer Git

[Git](https://git-scm.com/) est le système de gestion du contrôle de code source utilisé par [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html), et est donc nécessaire au développement.

+ Installation de Git à l’aide de Homebrew
   1. Ouvrez votre invite de commande/terminal.
   1. Exécutez la commande : `brew install git`
   1. Vérifiez que Git est installé à l’aide de la commande : `git --version`
+ Ou, téléchargez et installez Git (macOS, Linux ou Windows).
   1. [Télécharger et installer Git](https://git-scm.com/downloads)
   1. Ouvrez votre invite de commande/terminal.
   1. Vérifiez que Git est installé à l’aide de la commande : `git --version`

![Git](./assets/development-tools/git.png)

## Installation de Node.js (et npm){#node-js}

[Node.js](https://nodejs.org) est un environnement d’exécution JavaScript utilisé pour fonctionner avec les ressources front-end d’un projet AEM __ui.frontend__ sous-projet. Node.js est distribué avec [npm](https://www.npmjs.com/), est le gestionnaire de modules Node.js par défaut, utilisé pour gérer les dépendances JavaScript.

+ Installation de Node.js à l’aide de Homebrew
   1. Ouvrez votre invite de commande/terminal.
   1. Exécutez la commande : `brew install node`
   1. Vérifiez que Node.js est installé à l’aide de la commande : `node -v`
   1. Vérifiez que npm est installé à l’aide de la commande : `npm -v`
+ Ou, téléchargez et installez Node.js (macOS, Linux ou Windows).
   1. [Télécharger et installer Node.js](https://nodejs.org/en/download/)
   1. Ouvrez votre invite de commande/terminal.
   1. Vérifiez que Node.js est installé à l’aide de la commande : `node -v`
   1. Vérifiez que npm est installé à l’aide de la commande : `npm -v`

![Node.js et npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Archétype de projet](https://github.com/adobe/aem-project-archetype)AEM Projets basés sur , installez une version isolée de Node.js au moment de la création. Il est conseillé de conserver la version du système de développement local synchronisée (ou proche) des versions Node.js et npm spécifiées dans le fichier Reactor pom.xml de votre projet Maven AEM.
>
>Voir cet exemple [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) pour savoir où trouver les versions Node.js et npm build.

## Installer Maven

Apache Maven est l’outil de ligne de commande Java open source utilisé pour créer AEM projets générés à partir de l’archétype Maven de projet AEM. Tous les principaux IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) ont intégré la prise en charge de Maven.

+ Installation de Maven à l’aide de Homebrew
   1. Ouvrez votre invite de commande/terminal.
   1. Exécutez la commande : `brew install maven`
   1. Vérifiez que Maven est installé à l’aide de la commande : `mvn -v`
+ Ou, téléchargez et installez Maven (macOS, Linux ou Windows).
   1. [Télécharger Maven](https://maven.apache.org/download.cgi)
   1. [Installer Maven](https://maven.apache.org/install.html)
   1. Ouvrez votre invite de commande/terminal.
   1. Vérifiez que Maven est installé à l’aide de la commande : `mvn -v`

![Maven](./assets/development-tools/maven.png)

## Configuration de l’interface de ligne de commande Adobe I/O{#aio-cli}

Le [Interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli)ou `aio`, permet d’accéder à divers services Adobe en ligne de commande, notamment : [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) et [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). L’interface de ligne de commande d’Adobe I/O joue un rôle essentiel dans le développement sur AEM as a Cloud Service, car elle permet aux développeurs de :

+ Suivi des journaux à partir d’AEM as a Cloud Services services
+ Gestion des pipelines de Cloud Manager à partir de l’interface de ligne de commande

### Installation de l’interface de ligne de commande d’Adobe I/O

1. Assurez-vous que [Node.js est installé](#node-js) car l’interface de ligne de commande d’Adobe I/O est un module npm
   + Exécuter `node --version` confirmer
1. Exécuter `npm install -g @adobe/aio-cli` pour installer le `aio` module npm globalement

### Configuration du module externe Adobe I/O CLI Cloud Manager{#aio-cloud-manager}

Le module externe Adobe I/O Cloud Manager permet à l’interface de ligne de commande d’AEM d’interagir avec Adobe Cloud Manager via l’interface de ligne de commande `aio cloudmanager` .

1. Exécuter `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` pour installer le [module externe d’aio Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

### Configuration du module Adobe I/O d’Asset compute de l’interface de ligne de commande{#aio-asset-compute}

Le module externe Adobe I/O Cloud Manager permet à l’interface de ligne de commande d’aio de générer et d’exécuter des objets Worker Asset compute via l’interface de ligne de commande `aio asset-compute` .

1. Exécuter `aio plugins:install @adobe/aio-cli-plugin-asset-compute` pour installer le [module externe aio Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute).

### Configuration de l’authentification de l’interface de ligne de commande Adobe I/O

Pour que l’interface de ligne de commande de l’Adobe I/O communique avec Cloud Manager, une [L’intégration de Cloud Manager doit être créée dans Adobe I/O Console](https://github.com/adobe/aio-cli-plugin-cloudmanager), et les informations d’identification doivent être obtenues pour réussir l’authentification.

1. Connectez-vous à [console.adobe.io](https://console.adobe.io)
1. Assurez-vous que votre organisation qui comprend le produit Cloud Manager auquel se connecter est principale dans le sélecteur d’organisation Adobe
1. Créer ou ouvrir un [Programme Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Les programmes Adobe I/O Console sont simplement des regroupements organisationnels d’intégrations, de créer ou d’utiliser et de programmes existants en fonction de la manière dont vous souhaitez gérer vos intégrations.
   + Si vous créez un projet, sélectionnez &quot;Projet vide&quot; si vous y êtes invité (ou &quot;Créer à partir d’un modèle&quot;)
   + Les programmes Adobe I/O Console sont des concepts différents des programmes Cloud Manager.
1. Créez une nouvelle intégration de l’API Cloud Manager avec le profil &quot;Développeur - Cloud Service&quot;
1. L’obtention des informations d’identification du compte de service (JWT) doit être renseignée dans l’interface de ligne de commande d’Adobe I/O. [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. Chargez la variable `config.json` dans l’interface de ligne de commande de l’Adobe I/O.
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Chargez la variable `private.key` dans l’interface de ligne de commande de l’Adobe I/O.
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Début [exécution des commandes](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) pour Cloud Manager via l’interface de ligne de commande d’Adobe I/O.

## Configuration de l’IDE de développement

Le développement AEM consiste principalement en un développement Java et front-end (JavaScript, CSS, etc.) et en une gestion XML. Voici les IDE les plus populaires pour le développement AEM.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ est un IDE puissant pour le développement de Java. IntelliJ IDEA est disponible en deux versions, une édition communautaire gratuite et une version finale commerciale (payante). La version communautaire libre est suffisante pour AEM développement, mais la version Ultimate [développe son ensemble de fonctionnalités.](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [Télécharger IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Téléchargement de l’outil Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Code Visual Studio Microsoft

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code) est un outil libre et open source pour les développeurs front-end. Visual Studio Code peut être configuré pour intégrer la synchronisation de contenu avec AEM à l’aide d’un outil Adobe, __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code est le choix idéal pour les développeurs front-end qui créent principalement du code frontal ; JavaScript, CSS et HTML. Bien que VS Code dispose d’une prise en charge Java via [extensions](https://code.visualstudio.com/docs/java/java-tutorial), il peut ne pas disposer de certaines fonctionnalités avancées fournies par des fonctionnalités plus spécifiques à Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Télécharger le code Visual Studio](https://code.visualstudio.com/Download)
+ [Téléchargement de l’outil Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Télécharger l’extension aemfed VS Code](https://aemfed.io/)
+ [Télécharger l’extension AEM synchronisation VS Code](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ est un IDE populaire pour le développement de Java et prend en charge les  __[AEM Outils de développement](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ module externe fourni par Adobe, fournissant une interface utilisateur graphique intégrée pour la création et la synchronisation du contenu JCR avec une instance d’AEM locale.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Télécharger Eclipse](https://www.eclipse.org/ide/)
+ [Téléchargement des outils de développement Eclipse](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
