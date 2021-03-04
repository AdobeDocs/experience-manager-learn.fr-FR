---
title: Prise en main de AEM Sites - Configuration du projet
seo-title: Prise en main de AEM Sites - Configuration du projet
description: Couvre la création d'un projet Maven Multi Module pour gérer le code et les configurations d'un site AEM.
sub-product: sites
feature: Archétype de projet AEM
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
topic: '"Gestion de contenu, développement"'
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1895'
ht-degree: 6%

---


# Configuration du projet {#project-setup}

Ce didacticiel porte sur la création d&#39;un projet Maven Multi Module pour gérer le code et les configurations d&#39;un site Adobe Experience Manager.

## Conditions préalables {#prerequisites}

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment). Assurez-vous que vous disposez d’une nouvelle instance de Adobe Experience Manager disponible localement et qu’aucun exemple ou package de démonstration supplémentaire n’a été installé (autre que les Service Packs requis).

## Intention {#objective}

1. Découvrez comment générer un nouveau projet AEM à l’aide d’un archétype expert.
1. Comprendre les différents modules générés par l&#39;archétype de projet AEM et comment ils fonctionnent ensemble.
1. Comprendre comment AEM composants principaux sont inclus dans un projet AEM.

## Ce que vous allez créer {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

Dans ce chapitre, vous allez générer un nouveau projet Adobe Experience Manager à l&#39;aide de l&#39;[AEM archétype de projet](https://github.com/adobe/aem-project-archetype). Votre projet AEM contient tout le code, le contenu et les configurations utilisés pour une implémentation de sites. Le projet généré dans le présent chapitre servira de base à la mise en oeuvre du site WKND et sera développé dans les prochains chapitres.

**Qu&#39;est-ce qu&#39;un projet Maven ?** -  [Apache ](https://maven.apache.org/) Mavenis est un outil de gestion de logiciels pour construire des projets. *Toutes les implémentations d’Adobe Experience* Manager utilisent des projets Maven pour créer, gérer et déployer du code personnalisé en plus de l’AEM.

**Qu&#39;est-ce qu&#39;un archétype Maven ?** - Un  [archétype ](https://maven.apache.org/archetype/index.html) Maven est un modèle ou un modèle permettant de générer de nouveaux projets. L&#39;archétype du projet AEM nous permet de générer un nouveau projet avec un espace de nommage personnalisé et d&#39;inclure une structure de projet qui suit les meilleures pratiques, ce qui accélère considérablement notre projet.

## Créer le projet {#create}

Il existe quelques options pour créer un projet Maven Multi-module pour AEM. Ce didacticiel s&#39;appuie sur l&#39;archétype de projet [Maven AEM Project **25**](https://github.com/adobe/aem-project-archetype). Cloud Manager [fournit également un assistant d’interface utilisateur](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) pour lancer la création d’un projet d’application AEM. Le projet sous-jacent généré par l’interface utilisateur de Cloud Manager génère la même structure que l’utilisation directe de l’archétype.

>[!NOTE]
>
>Ce didacticiel utilise la version **25** de l&#39;archétype. Il est toujours recommandé d’utiliser la version **la plus récente** de l’archétype pour générer un nouveau projet.

La prochaine série d&#39;étapes aura lieu à l&#39;aide d&#39;un terminal de ligne de commande UNIX, mais elle doit être similaire si vous utilisez un terminal Windows.

1. Ouvrez un terminal de ligne de commande. Vérifiez que Maven est installé :

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. Vérifiez que le profil **adobe-public** est principal en exécutant la commande suivante :

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Si vous **ne consultez pas** le **adobe-public**, cela indique que le référentiel d’Adobe n’est pas référencé correctement dans votre fichier `~/.m2/settings.xml`. Veuillez revoir les étapes d&#39;installation et de configuration d&#39;Apache Maven dans [un environnement de développement local](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Accédez au répertoire dans lequel vous souhaitez générer le projet AEM. Il peut s’agir de n’importe quel répertoire dans lequel vous souhaitez conserver le code source de votre projet. Par exemple, un répertoire nommé `code` sous le répertoire racine de l’utilisateur :

   ```shell
   $ cd ~/code
   ```

1. Collez les éléments suivants dans la ligne de commande pour [générer le projet en mode batch](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html) :

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=25 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Si vous utilisez AEM 6.5.5.0+ ou 6.4.8.1+, remplacez `aemVersion="cloud"` par votre version cible de AEM, c’est-à-dire `aemVersion="6.5.5"` ou `aemVersion="6.4.8.1"`.

   Une liste complète des propriétés disponibles pour la configuration d&#39;un projet [se trouve ici](https://github.com/adobe/aem-project-archetype#available-properties).

1. La structure de dossiers et de fichiers suivante sera générée par l’archétype Maven sur votre système de fichiers local :

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## Déploiement et génération du projet {#build}

Créez et déployez le code du projet sur une instance locale d&#39;AEM.

1. Vérifiez que vous disposez d’une instance d’auteur d’AEM s’exécutant localement sur le port **4502**.
1. Dans la ligne de commande, accédez au répertoire du projet `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Exécutez la commande suivante pour générer et déployer le projet entier sur AEM :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   La compilation prend environ une minute et doit se terminer par le message suivant :

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Le profil Maven `autoInstallSinglePackage` compile les modules individuels du projet et déploie un package unique sur l’instance AEM. Par défaut, ce package sera déployé sur une instance AEM s&#39;exécutant localement sur le port **4502** et avec les informations d&#39;identification `admin:admin`.

1. Accédez à Package Manager sur votre instance d’AEM locale : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Vous devriez voir les packages pour `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` et `aem-guides-wknd.all`.

1. Accédez à la console Sites : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Le site WKND sera l&#39;un des sites. Il comprendra une structure de site avec une hiérarchie de Principal de langue et de langue. Cette hiérarchie de site est basée sur les valeurs de `language_country` et `isSingleCountryWebsite` lors de la génération du projet à l&#39;aide de l&#39;archétype.

1. Ouvrez la page **FR** `>` **Anglais** en sélectionnant la page et en cliquant sur le bouton **Modifier** dans la barre de menus :

   ![console du site](assets/project-setup/aem-sites-console.png)

1. Le contenu de démarrage a déjà été créé et plusieurs composants peuvent être ajoutés à une page. Testez ces composants pour avoir une idée de la fonctionnalité. Vous apprendrez les bases d’un composant dans le chapitre suivant.

   ![Home Starter Content](assets/project-setup/start-home-page.png)

   *Exemple de contenu généré par l&#39;archétype*

## Inspect le projet {#project-structure}

Le projet AEM généré est composé de modules Maven individuels, chacun ayant un rôle différent. Ce didacticiel et la majorité des développements portent sur ces modules :

* [noyau](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)  - Code Java, principalement les développeurs principaux.
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)  : contient le code source pour CSS, JavaScript, Sass, Type Script, principalement pour les développeurs frontaux.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  : contient des définitions de composants et de boîtes de dialogue, incorpore les fichiers CSS compilés et JavaScript en tant que bibliothèques client.
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) - contient du contenu structurel et des configurations telles que des modèles modifiables, des schémas de métadonnées (/content, /conf).

* **tout**  : il s&#39;agit d&#39;un module Maven vide qui combine les modules ci-dessus en un seul package pouvant être déployé sur un environnement AEM.

![Maven Project Diagramme](assets/project-setup/project-pom-structure.png)

Consultez la [documentation de l&#39;archétype de projet ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) pour en savoir plus sur **tous** les modules Maven.

### Inclusion des composants de base {#core-components}

[AEM ](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html) Composants principaux sont un ensemble de composants de Gestion de contenu Web normalisés (WCM) pour AEM. Ces composants fournissent un ensemble de base de fonctionnalités et sont conçus pour être mis en forme, personnalisés et étendus pour des projets individuels.

AEM en tant qu&#39;environnements Cloud Service incluent la dernière version de [AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html). Par conséquent, les projets générés pour l&#39;AEM en tant que Cloud Service ne **pas** incluent une intégration des AEM principaux composants.

Pour les projets générés AEM 6.5/6.4, l&#39;archétype incorpore automatiquement [AEM Composants principaux ](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) dans le projet. AEM 6.5/6.4 recommande d’incorporer AEM composants principaux afin de garantir que la dernière version est déployée avec votre projet. Vous trouverez plus d&#39;informations sur la façon dont les composants principaux sont [inclus dans le projet ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Gestion du contrôle de code source {#source-control}

Il est toujours conseillé d’utiliser une forme de contrôle de code source pour gérer le code dans votre application. Ce didacticiel utilise git et GitHub. Il existe plusieurs fichiers générés par Maven et/ou l&#39;IDE de choix qui doivent être ignorés par le SCM.

Maven crée un dossier de cible chaque fois que vous créez et installez le package de code. Le dossier et le contenu de la cible doivent être exclus de SCM.

Sous `ui.apps`, observez que de nombreux fichiers `.content.xml` sont créés. Ces fichiers XML mappent les types de noeud et les propriétés du contenu installé dans le JCR. Ces fichiers sont essentiels et **ne doivent pas être ignorés**.

L&#39;archétype de projet AEM génère un fichier d&#39;exemple `.gitignore` qui peut être utilisé comme point de départ pour lequel les fichiers peuvent être ignorés en toute sécurité. Le fichier est généré à `<src>/aem-guides-wknd/.gitignore`.

## Félicitations! {#congratulations}

Félicitations, vous venez de créer votre premier projet AEM !

### Étapes suivantes {#next-steps}

Comprenez la technologie sous-jacente d&#39;un composant Sites Adobe Experience Manager (AEM) à l&#39;aide d&#39;un exemple simple `HelloWorld` avec le didacticiel [Component Basics](component-basics.md).

## Commandes Maven avancées (Bonus) {#advanced-maven-commands}

Au cours du développement, vous pouvez travailler avec un seul des modules et vous souhaitez éviter de construire l&#39;ensemble du projet afin de gagner du temps. Vous pouvez également effectuer un déploiement direct sur une instance AEM Publish ou peut-être sur une instance d’AEM non exécutée sur le port 4502.

Nous allons maintenant examiner quelques profils et commandes Maven supplémentaires que vous pouvez utiliser pour une plus grande flexibilité lors du développement.

### Module principal {#core-module}

Le module **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contient tout le code Java associé au projet. Lorsqu’il est créé, il déploie un lot OSGi vers AEM. Pour créer uniquement ce module :

1. Accédez au dossier `core` (sous `aem-guides-wknd`) :

   ```shell
   $ cd core/
   ```

1. Exécutez la commande suivante :

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. Accédez à [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Il s&#39;agit de la console Web OSGi et contient des informations sur tous les lots installés sur l&#39;instance AEM.

1. Basculez sur la colonne de tri **Id** et vous devriez voir le lot WKND installé et principal.

   ![Offre principale](assets/project-setup/wknd-osgi-console.png)

1. Vous pouvez voir l’emplacement &quot;physique&quot; du fichier jar dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar) :

   ![Emplacement CRXDE de Jar](assets/project-setup/jcr-bundle-location.png)

### Modules Ui.apps et Ui.content {#apps-content-module}

Le module expert **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contient tout le code de rendu nécessaire pour le site sous `/apps`. Ce module comprend les éléments CSS/JS qui seront stockés dans un format AEM appelé [clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html). Cela inclut également les scripts [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/overview.html) pour le rendu du code HTML dynamique. Vous pouvez considérer le module **ui.apps** comme une carte de la structure dans le JCR, mais dans un format qui peut être stocké sur un système de fichiers et dédié au contrôle de code source. Le module **ui.apps** contient uniquement du code.

Pour créer le module suivant :

1. Dans la ligne de commande. Accédez au dossier `ui.apps` (sous `aem-guides-wknd`) :

   ```shell
   $ cd ../ui.apps
   ```

1. Exécutez la commande suivante :

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Accédez à [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Vous devriez voir le package `ui.apps` comme le premier package installé et il doit avoir un horodatage plus récent que tous les autres packages.

   ![Package Ui.apps installé](assets/project-setup/ui-apps-package.png)

1. Revenez à la ligne de commande et exécutez la commande suivante (dans le dossier `ui.apps`) :

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Le profil `autoInstallPackagePublish` est destiné à déployer le package sur un environnement de publication s’exécutant sur le port **4503**. L’erreur ci-dessus est attendue si une instance AEM s’exécutant sur http://localhost:4503 est introuvable.

1. Exécutez enfin la commande suivante pour déployer le package `ui.apps` sur le port **4504** :

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   Là encore, une erreur de génération est attendue si aucune instance AEM s&#39;exécutant sur le port **4504** n&#39;est disponible. Le paramètre `aem.port` est défini dans le fichier POM à `aem-guides-wknd/pom.xml`.

Le module **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** est structuré de la même manière que le module **ui.apps**. La seule différence est que le module **ui.content** contient ce que l&#39;on appelle le contenu **mutable**. **Le** contenu enchaîné fait essentiellement référence à des configurations non codées telles que les modèles, les stratégies ou les structures de dossiers qui sont stockées dans le contrôle de code source  **** mais qui peuvent être modifiées directement sur une instance AEM. Ce point sera traité de manière beaucoup plus détaillée dans le chapitre sur les pages et les modèles.

Les mêmes commandes Maven utilisées pour construire le module **ui.apps** peuvent être utilisées pour construire le module **ui.content**. N’hésitez pas à répéter les étapes ci-dessus depuis le dossier **ui.content**.