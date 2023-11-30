---
title: Configurer des outils de développement pour le développement AEM as a Cloud Service
description: Configurez une machine de développement locale avec tous les outils de base nécessaires pour le développement local avec AEM.
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1483'
ht-degree: 100%

---

# Configurer des outils de développement {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="Configurer des outils de développement"
>abstract="Le développement d’Adobe Experience Manager (AEM) nécessite l’installation et la configuration d’un ensemble minimal d’outils de développement sur la machine du développeur ou de la développeuse. Ces outils comprennent Java, Maven, Adobe I/O CLI, Development IDE, etc."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=fr" text="Consignes de développement"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=fr" text="Principes de base de développement"

Le développement d’Adobe Experience Manager (AEM) nécessite l’installation et la configuration d’un jeu minimal d’outils de développement sur la machine du développeur ou de la développeuse. Ces outils prennent en charge le développement et la création de projets AEM.

Notez que `~` est utilisé comme raccourci pour le répertoire de l’utilisateur ou de l’utilisatrice. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

## Installer Java

Experience Manager est une application Java qui requiert donc le SDK Java pour prendre en charge le développement et le SDK AEM as a Cloud Service.

1. [Télécharger et installer la dernière version du SDK Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Vérifiez que le SDK Oracle Java 11 est installé en exécutant la commande :

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java.](./assets/development-tools/java.png)

## Installer Homebrew

_L’utilisation de Homebrew est facultative, mais recommandée._

Homebrew est un gestionnaire de packages Open Source pour macOS, Windows et Linux. Tous les outils de prise en charge peuvent être installés séparément. Homebrew offre un moyen pratique d’installer et de mettre à jour divers outils de développement requis pour le développement d’Experience Manager.

1. Ouvrez votre terminal.
1. Vérifiez si Homebrew est déjà installé en exécutant la commande : `brew --version`.
1. Si Homebrew n’est pas installé, installez Homebrew.

>[!BEGINTABS]

>[!TAB macOS]

[Homebrew sur macOS](https://brew.sh/) nécessite [Xcode](https://apps.apple.com/us/app/xcode/id497799835) ou les [outils de ligne de commande](https://developer.apple.com/download/more/), qui peuvent être installés via la commande :

```shell
$ xcode-select --install
```

>[!TAB Windows]

[Installer Homebrew sous Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[Installer Homebrew sous Linux](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. Vérifiez que Homebrew est installé en exécutant la commande : `brew --version`.

![Homebrew.](./assets/development-tools/homebrew.png)

Si vous utilisez Homebrew, suivez les instructions __Installation à l’aide de Homebrew__ dans les sections ci-dessous. Si vous n’utilisez __pas__ Homebrew, installez les outils à l’aide des liens spécifiques au système d’exploitation.

## Installer Git

[Git](https://git-scm.com/) est le système de gestion du contrôle de code source utilisé par [Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html?lang=fr), et est donc nécessaire au développement.

>[!BEGINTABS]

>[!TAB Installer Git à l’aide de Homebrew]

1. Ouvrez votre invite de commande/terminal.
1. Exécutez la commande : `$ brew install git`
1. Vérifiez que Git est installé en exécutant la commande : `$ git --version`.

>[!TAB Télécharger et installer Git]

1. [Télécharger et installer Git](https://git-scm.com/downloads)
1. Ouvrez votre invite de commande/terminal.
1. Vérifiez que Git est installé en exécutant la commande : `$ git --version`.

>[!ENDTABS]

![Git.](./assets/development-tools/git.png)

## Installer Node.js (et npm){#node-js}

[Node.js](https://nodejs.org) est un environnement d’exécution JavaScript utilisé pour fonctionner avec les ressources front-end du sous-projet __ui.frontend__ d’un projet AEM. Node.js est distribué avec [npm](https://www.npmjs.com/), qui est de facto le gestionnaire de packages Node.js utilisé pour gérer les dépendances JavaScript.

>[!BEGINTABS]

>[!TAB Installer Node.js à l’aide de Homebrew]

1. Ouvrez votre invite de commande/terminal.
1. Exécutez la commande : `$ brew install node`
1. Vérifiez que Node.js est installé à l’aide de la commande : `$ node -v`.
1. Vérifiez que npm est installé à l’aide de la commande : `$ npm -v`.

>[!TAB Télécharger et installer Node.js]

1. [Télécharger et installer Node.js](https://nodejs.org/fr/download/)
2. Ouvrez votre invite de commande/terminal.
3. Vérifiez que Node.js est installé à l’aide de la commande : `$ node -v`.
4. Vérifiez que npm est installé à l’aide de la commande : `$ npm -v`.

>[!ENDTABS]

![Node.js et npm.](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>Les projets AEM basés sur l’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype) installent une version isolée de Node.js au moment de la création. Il est conseillé de conserver la version du système de développement local synchronisée (ou proche) des versions Node.js et npm spécifiées dans le fichier Reactor pom.xml de votre projet Maven AEM.
>
>Voir cet exemple [fichier Reactor pom.xml de projet AEM](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) pour savoir où trouver les versions build Node.js et npm.

## Installer Maven

Apache Maven est l’outil de ligne de commande Java open source utilisé pour créer des projets AEM générés à partir de l’archétype Maven de projet AEM. Tous les principaux IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio Code](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/), etc.) ont intégré la prise en charge de Maven.


>[!BEGINTABS]

>[!TAB Installer Maven à l’aide de Homebrew]

1. Ouvrez votre invite de commande/terminal.
1. Exécutez la commande : `$ brew install maven`
1. Vérifiez que Maven est installé à l’aide de la commande : `$ mvn -v`

>[!TAB Télécharger et installer Maven]

1. [Télécharger Maven](https://maven.apache.org/download.cgi)
1. [Installer Maven](https://maven.apache.org/install.html)
1. Ouvrez votre invite de commande/terminal.
1. Vérifiez que Maven est installé à l’aide de la commande : `$ mvn -v`

>[!ENDTABS]

![Maven.](./assets/development-tools/maven.png)

## Configurer l’interface de ligne de commande Adobe I/O{#aio-cli}

L’[Interface de ligne de commande Adobe I/O](https://github.com/adobe/aio-cli), ou `aio`, permet d’accéder à divers services Adobe en ligne de commande, notamment : [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) et [Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute). L’interface de ligne de commande d’Adobe I/O joue un rôle essentiel dans le développement sur AEM as a Cloud Service, car elle permet aux développeurs et aux développeuses de :

+ Suivre des journaux à partir des services AEM as a Cloud Services.
+ Gérer des pipelines de Cloud Manager à partir de l’interface de ligne de commande.
+ Effectuer le déploiement sur des [environnements de développement rapide AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=fr).

### Installer l’interface de ligne de commande d’Adobe I/O

1. Assurez-vous que [Node.js est installé](#node-js), car l’interface de ligne de commande Adobe I/O est un module npm.
   + Exécutez `node --version` pour confirmer.
1. Exécutez `npm install -g @adobe/aio-cli` pour installer le module npm `aio` globalement.

### Configurer l’interface de ligne de commande d’Adobe I/O avec le plug-in Cloud Manager{#aio-cloud-manager}

Le plug-in Adobe I/O Cloud Manager permet à l’interface de ligne de commande d’Adobe I/O d’interagir avec Adobe Cloud Manager via la commande `aio cloudmanager`.

1. Exécutez `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` pour installer le [plug-in Adobe I/O Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### Configurer l’authentification de l’interface de ligne de commande d’Adobe I/O

Pour que l’interface de ligne de commande d’Adobe I/O communique avec Cloud Manager, une [intégration de Cloud Manager doit être créée dans la console Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager), et les informations d’identification doivent être obtenues pour une authentification réussie.

1. Connectez-vous à [console.adobe.io](https://console.adobe.io).
1. Assurez-vous que votre organisation qui comprend le produit Cloud Manager auquel se connecter est active dans le sélecteur d’organisation Adobe.
1. Ouvrez un [programme Adobe I/O](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md) existant ou créez-en un.
   + Les programmes de console Adobe I/O sont simplement des regroupements organisationnels d’intégrations. Créez ou utilisez un programme existant en fonction de la manière dont vous souhaitez gérer vos intégrations.
   + Si vous créez un projet, sélectionnez « Projet vide », le cas échéant (au lieu de « Créer à partir d’un modèle »).
   + Les programmes de console Adobe I/O sont des concepts différents des programmes Cloud Manager.
1. Créer une intégration de l’API Cloud Manager avec le profil « Développement - Cloud Service »
1. Obtenez les informations d’identification du compte de service (JWT) nécessaires pour remplir le fichier [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication) de l’interface de ligne de commande d’Adobe I/O.
1. Chargez le fichier `config.json` dans l’interface de ligne de commande d’Adobe I/O.
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. Chargez le fichier `private.key` dans l’interface de ligne de commande d’Adobe I/O.
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

Commencez à [exécuter des commandes](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) pour Cloud Manager via l’interface de ligne de commande d’Adobe I/O.

### Configurer le plug-in Environnements de développement rapide AEM{#rde}

Le plug-in Environnements de développement rapide AEM permet à l’interface de ligne de commande d’AIO d’interagir avec les [environnements de développement rapide](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=fr) AEM as a Cloud Service via la commande `aio aem:rde`.

1. Exécutez `aio plugins:install @adobe/aio-cli-plugin-aem-rde` pour installer le [plug-in Environnements de développement rapide AEM](https://github.com/adobe/aio-cli-plugin-aem-rde).

### Configurer le plug-in Asset Compute de l’interface de ligne de commande d’Adobe I/O{#aio-asset-compute}

Le plug-in Cloud Manager d’Adobe I/O permet à l’interface de ligne de commande d’AIO de générer et d’exécuter des programmes de travail Asset Compute via la commande `aio asset-compute`.

1. Exécutez `aio plugins:install @adobe/aio-cli-plugin-asset-compute` pour installer le [plug-in Asset Compute d’AIO](https://github.com/adobe/aio-cli-plugin-asset-compute).

## Configurer l’IDE de développement

Le développement AEM consiste principalement en un développement Java et front-end (JavaScript, CSS, etc.) et en une gestion XML. Voici les IDE les plus appréciés pour le développement AEM.

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ est un IDE puissant pour le développement Java. IntelliJ IDEA est disponible en deux versions, une édition Community gratuite et une version Ultimate commerciale (payante). La version Community gratuite est suffisante pour le développement AEM, mais la version Ultimate [élargit ses fonctionnalités.](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [Télécharger IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [Télécharger l’outil Repo Tool](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code) est un outil gratuit et open source pour les développeurs et les développeuses front-end. Visual Studio Code peut être configuré pour intégrer la synchronisation de contenu avec AEM à l’aide d’un outil Adobe, __[Repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code est le choix idéal pour les développeurs et les développeuses front-end qui créent principalement du code front-end : JavaScript, CSS et HTML. Bien que VS Code dispose d’une prise en charge Java via les [extensions](https://code.visualstudio.com/docs/java/java-tutorial), il peut ne pas disposer de certaines fonctionnalités avancées fournies par des outils plus spécifiques à Java.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [Télécharger Visual Studio Code](https://code.visualstudio.com/Download)
+ [Télécharger l’outil Repo Tool](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [Télécharger l’extension VS Code aemfd](https://aemfed.io)
+ [Télécharger l’extension VS Code AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ est un IDE populaire pour le développement Java et prend en charge le plug-in __[Outils de développement AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=fr)__ fourni par Adobe, fournissant une interface utilisateur graphique intégrée pour la création et la synchronisation du contenu JCR avec une instance AEM locale.

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [Télécharger Eclipse](https://www.eclipse.org/ide/)
+ [Télécharger les outils de développement Eclipse](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=fr)
