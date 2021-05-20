---
title: Prise en main d’AEM Sites - Configuration du projet
seo-title: Prise en main d’AEM Sites - Configuration du projet
description: Couvre la création d’un projet Maven multi-module pour gérer le code et les configurations d’un site AEM.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Archétype de projet AEM
topic: Gestion de contenu, développement
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1888'
ht-degree: 6%

---


# Configuration du projet {#project-setup}

Ce tutoriel porte sur la création d’un projet Maven multi-module pour gérer le code et les configurations d’un site Adobe Experience Manager.

## Prérequis {#prerequisites}

Examinez les outils et instructions requis pour configurer un [environnement de développement local](../overview.md#local-dev-environment). Assurez-vous qu’une nouvelle instance d’Adobe Experience Manager est disponible localement et qu’aucun exemple/module de démonstration supplémentaire n’a été installé (hormis les Service Packs requis).

## Intention {#objective}

1. Découvrez comment générer un nouveau projet AEM à l’aide d’un archétype Maven.
1. Découvrez les différents modules générés par l’archétype de projet AEM et comment ils fonctionnent ensemble.
1. Découvrez comment les composants principaux AEM sont inclus dans un projet AEM.

## Ce que vous allez créer {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

Dans ce chapitre, vous allez générer un nouveau projet Adobe Experience Manager à l’aide de l’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype). Votre projet AEM contient l’ensemble du code, du contenu et des configurations utilisés pour une mise en oeuvre de Sites. Le projet généré dans ce chapitre servira de base à une mise en oeuvre du site WKND et sera développé dans les prochains chapitres.

**Qu’est-ce qu’un projet Maven ?** -  [Apache ](https://maven.apache.org/) Mavenis est un outil de gestion logicielle pour la création de projets. *Toutes les mises en oeuvre d’Adobe Experience* Manager utilisent des projets Maven pour créer, gérer et déployer du code personnalisé en plus des AEM.

**Qu’est-ce qu’un archétype Maven ?** - Un  [ ](https://maven.apache.org/archetype/index.html) archétype Maven est un modèle ou un modèle permettant de générer de nouveaux projets. L’archétype de projet AEM nous permet de générer un nouveau projet avec un espace de noms personnalisé et d’inclure une structure de projet qui suit les bonnes pratiques, ce qui accélère considérablement notre projet.

## Création du projet {#create}

Il existe plusieurs options pour créer un projet Maven multi-module pour AEM. Ce tutoriel utilisera l’[archétype de projet Maven AEM **26**](https://github.com/adobe/aem-project-archetype). Cloud Manager fournit également [un assistant d’interface utilisateur](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) pour lancer la création d’un projet d’application AEM. Le projet sous-jacent généré par l’interface utilisateur de Cloud Manager donne la même structure que l’utilisation directe de l’archétype.

>[!NOTE]
>
>Ce tutoriel utilise la version **26** de l’archétype. Il est toujours recommandé d’utiliser la version **la plus récente** de l’archétype pour générer un nouveau projet.

La série d’étapes suivante s’effectuera à l’aide d’un terminal de ligne de commande UNIX, mais elle doit être similaire si vous utilisez un terminal Windows.

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

   Si vous ne **pas** voir **adobe-public**, cela indique que le référentiel d’Adobe n’est pas correctement référencé dans votre fichier `~/.m2/settings.xml`. Veuillez revoir les étapes d’installation et de configuration d’Apache Maven dans [un environnement de développement local](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. Accédez au répertoire dans lequel vous souhaitez générer le projet AEM. Il peut s’agir de n’importe quel répertoire dans lequel vous souhaitez gérer le code source de votre projet. Par exemple, un répertoire nommé `code` sous le répertoire racine de l’utilisateur :

   ```shell
   $ cd ~/code
   ```

1. Collez les éléments suivants dans la ligne de commande pour [générer le projet en mode batch](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html) :

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=26 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > Si le ciblage AEM 6.5.5+, remplacez `aemVersion="cloud"` par `aemVersion="6.5.5"`. Si le ciblage est version 6.4.8+, utilisez `aemVersion="6.4.8"`.

   Une liste complète des propriétés disponibles pour la configuration d’un projet [se trouve ici](https://github.com/adobe/aem-project-archetype#available-properties).

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

## Déployer et créer le projet {#build}

Créez et déployez le code du projet sur une instance locale d’AEM.

1. Vérifiez que vous disposez d’une instance d’auteur d’AEM s’exécutant localement sur le port **4502**.
1. À partir de la ligne de commande, accédez au répertoire du projet `aem-guides-wknd`.

   ```shell
   $ cd aem-guides-wknd
   ```

1. Exécutez la commande suivante pour créer et déployer l’ensemble du projet vers AEM :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Le build prend environ une minute et doit se terminer par le message suivant :

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

   Le profil Maven `autoInstallSinglePackage` compile les modules individuels du projet et déploie un seul module sur l’instance AEM. Par défaut, ce package sera déployé sur une instance AEM s’exécutant localement sur le port **4502** et avec les informations d’identification `admin:admin`.

1. Accédez au gestionnaire de modules sur votre instance d’AEM locale : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Vous devriez voir les modules pour `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content` et `aem-guides-wknd.all`.

1. Accédez à la console Sites : [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). Le site WKND sera l’un des sites. Il comprendra une structure de site avec une hiérarchie de Principal des États-Unis et de la langue. Cette hiérarchie de site est basée sur les valeurs `language_country` et `isSingleCountryWebsite` lors de la génération du projet à l’aide de l’archétype.

1. Ouvrez la page **US** `>` **Anglais** en sélectionnant la page et en cliquant sur le bouton **Modifier** dans la barre de menus :

   ![console du site](assets/project-setup/aem-sites-console.png)

1. Le contenu de démarrage a déjà été créé et plusieurs composants peuvent être ajoutés à une page. Testez ces composants pour en avoir une idée. Vous apprendrez les principes de base d’un composant dans le chapitre suivant.

   ![Home Starter Content](assets/project-setup/start-home-page.png)

   *Exemple de contenu généré par l’archétype*

## Inspect du projet {#project-structure}

Le projet d’AEM généré est constitué de modules Maven individuels, chacun avec un rôle différent. Ce tutoriel et la plupart des développements portent sur ces modules :

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)  : code Java, principalement pour les développeurs principaux.
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)  : contient le code source pour CSS, JavaScript, Sass, Type Script, principalement pour les développeurs front-end.
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)  : contient des définitions de composant et de boîte de dialogue, incorpore les fichiers CSS et JavaScript compilés en tant que bibliothèques clientes.
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html)  : contient du contenu structurel et des configurations telles que des modèles modifiables, des schémas de métadonnées (/content, /conf).

* **all**  : il s’agit d’un module Maven vide qui combine les modules ci-dessus dans un module unique pouvant être déployé dans un environnement AEM.

![Diagramme de projet Maven](assets/project-setup/project-pom-structure.png)

Pour plus d’informations sur **tous** les modules Maven, reportez-vous à la [documentation AEM de l’archétype de projet](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) .

### Inclusion des composants principaux {#core-components}

[AEM ](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html) les composants principaux sont un ensemble de composants WCM (Web Content Management, gestion de contenu web) normalisés pour AEM. Ces composants fournissent un ensemble de base de fonctionnalités et sont conçus pour être stylisés, personnalisés et étendus pour des projets individuels.

Les environnements AEM as a Cloud Service incluent la dernière version de [AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html). Par conséquent, les projets générés pour AEM en tant que Cloud Service ne **pas** incluent une intégration des composants principaux AEM.

Pour les projets générés par AEM 6.5/6.4, l’archétype incorpore automatiquement [AEM Core Components](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html) dans le projet. Il est recommandé d’incorporer AEM composants principaux dans AEM version 6.5/6.4 afin de garantir que la dernière version est déployée avec votre projet. Vous trouverez plus d’informations sur la façon dont les composants principaux sont [inclus dans le projet ici](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## Gestion du contrôle de code source {#source-control}

Il est toujours préférable d’utiliser une forme de contrôle source pour gérer le code dans votre application. Ce tutoriel utilise git et GitHub. Il existe plusieurs fichiers générés par Maven et/ou l’IDE de votre choix qui doivent être ignorés par le SCM.

Maven crée un dossier cible chaque fois que vous créez et installez le module de code. Le dossier cible et son contenu doivent être exclus de SCM.

Sous `ui.apps` , notez que de nombreux fichiers `.content.xml` sont créés. Ces fichiers XML mappent les types de noeuds et les propriétés du contenu installé dans le JCR. Ces fichiers sont essentiels et **et** ne doivent pas être ignorés.

L’archétype de projet AEM génère un fichier `.gitignore` d’exemple qui peut être utilisé comme point de départ pour lequel les fichiers peuvent être ignorés en toute sécurité. Le fichier est généré à `<src>/aem-guides-wknd/.gitignore`.

## Félicitations !  {#congratulations}

Félicitations, vous venez de créer votre premier projet AEM !

### Étapes suivantes {#next-steps}

Découvrez la technologie sous-jacente d’un composant Sites Adobe Experience Manager (AEM) à l’aide d’un simple exemple `HelloWorld` du tutoriel [Notions de base des composants](component-basics.md).

## Commandes Maven avancées (Bonus) {#advanced-maven-commands}

Pendant le développement, vous pouvez travailler avec un seul des modules et éviter de créer l’ensemble du projet afin de gagner du temps. Vous pouvez également effectuer un déploiement direct sur une instance AEM Publish ou peut-être sur une instance d’AEM non exécutée sur le port 4502.

Ensuite, nous allons examiner quelques commandes et profils Maven supplémentaires que vous pouvez utiliser pour une plus grande flexibilité lors du développement.

### Module principal {#core-module}

Le module **[core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** contient tout le code Java associé au projet. Une fois créé, il déploie un lot OSGi vers AEM. Pour créer uniquement ce module :

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

1. Accédez à [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). Il s’agit de la console web OSGi qui contient des informations sur tous les lots installés sur l’instance AEM.

1. Activez/désactivez la colonne de tri **Id** et vous devriez voir le lot WKND installé et principal.

   ![Groupe principal](assets/project-setup/wknd-osgi-console.png)

1. Vous pouvez voir l’emplacement &quot;physique&quot; du fichier jar dans [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar) :

   ![Emplacement CRXDE du fichier Jar](assets/project-setup/jcr-bundle-location.png)

### Modules Ui.apps et Ui.content {#apps-content-module}

Le module Maven **[ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)** contient tout le code de rendu nécessaire au site situé sous `/apps`. Ce module comprend les éléments CSS/JS qui seront stockés dans un format AEM appelé [clientlibs](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/clientlibs.html). Cela inclut également les scripts [HTL](https://docs.adobe.com/content/help/fr-FR/experience-manager-htl/using/overview.html) pour le rendu du code HTML dynamique. Vous pouvez considérer le module **ui.apps** comme un mappage à la structure dans le JCR, mais dans un format pouvant être stocké sur un système de fichiers et validé dans le contrôle source. Le module **ui.apps** contient uniquement du code.

Pour créer le module proprement dit :

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

1. Revenez à la ligne de commande et exécutez la commande suivante (dans le dossier `ui.apps` ) :

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

   Le profil `autoInstallPackagePublish` est destiné à déployer le package dans un environnement de publication s’exécutant sur le port **4503**. L’erreur ci-dessus est attendue si une instance AEM s’exécutant sur http://localhost:4503 est introuvable.

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

   Une nouvelle fois, un échec de génération est attendu si aucune instance AEM s’exécutant sur le port **4504** n’est disponible. Le paramètre `aem.port` est défini dans le fichier POM à l’adresse `aem-guides-wknd/pom.xml`.

Le module **[ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** est structuré de la même manière que le module **ui.apps** . La seule différence est que le module **ui.content** contient ce que l’on appelle du contenu **mutable**. **** Le contenu cumulé fait essentiellement référence à des configurations non codées telles que des modèles, des stratégies ou des structures de dossiers stockées dans le contrôle source,  **** mais qui peuvent être modifiées directement sur une instance AEM. Ce point sera traité plus en détail dans le chapitre Pages et modèles.

Les mêmes commandes Maven que celles utilisées pour créer le module **ui.apps** peuvent être utilisées pour créer le module **ui.content**. N’hésitez pas à répéter les étapes ci-dessus depuis le dossier **ui.content** .
