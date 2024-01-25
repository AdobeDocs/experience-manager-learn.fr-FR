---
title: Configurer un environnement de développement AEM local
description: Découvrez comment configurer un environnement de développement local pour Experience Manager. Découvrez l’installation locale, Apache Maven, les environnements de développement intégrés, le débogage et la résolution des problèmes. Utilisez Eclipse IDE, CRXDE-Lite, Visual Studio Code et IntelliJ.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4612
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 100%

---

# Configurer un environnement de développement AEM local

Guide de configuration d’un développement local pour Adobe Experience Manager, AEM. Cet article présente l’installation locale, Apache Maven, les environnements de développement intégrés, le débogage et la résolution de problèmes. Le développement avec **Eclipse IDE, CRXDE Lite, Visual Studio Code et IntelliJ** est également abordé.

## Vue d’ensemble

La configuration d’un environnement de développement local constitue la première étape du développement pour Adobe Experience Manager ou AEM. Prenez le temps de configurer un environnement de développement de qualité afin d’augmenter votre productivité et d’écrire un meilleur code plus rapidement. L’environnement de développement local AEM consiste en quatre zones :

* Instances AEM locales
* Projet [!DNL Apache Maven]
* Environnements de développement intégrés (IDE)
* Résolution des problèmes

## Installer des instances AEM locales

Lorsque nous nous référons à une instance AEM locale, il s’agit d’une copie d’Adobe Experience Manager exécutée sur la machine personnelle d’un développeur ou d’une développeuse. ***Tout*** effort de développement AEM doit commencer par l’écriture et l’exécution de code sur une instance AEM locale.

Si vous découvrez AEM, sachez que deux modes d’exécution de base peuvent être installés : ***Création*** et ***Publication***. Le [mode d’exécution](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=fr) de ***Création*** est l’environnement utilisé par les personnes chargées du marketing numérique pour créer et gérer du contenu. Lors du développement, vous déployez la plupart du temps du code sur une instance de création. Vous pouvez ainsi créer des pages, ajouter et configurer des composants. AEM Sites est un CMS de création WYSIWYG. La plupart des codes CSS et JavaScript peuvent donc être testés sur une instance de création.

Il est également *essentiel* de tester le code sur une instance de ***Publication*** locale. L’instance de ***Publication*** est l’environnement AEM dans lequel les visiteurs et visiteuses de votre site web interagissent. Bien que l’instance de ***Publication*** repose sur la même pile technologique que l’instance de ***Création***, des différences majeures apparaissent entre les configurations et les autorisations. Le code doit être testé sur une instance de ***Publication*** avant d’aller plus loin.

### Étapes

1. Assurez-vous que Java™ est installé.
   * [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) est conseillé pour AEM 6.5 et versions ultérieures.
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) est adapté aux versions AEM antérieures à AEM 6.5.
1. Récupérez une copie du fichier [Jar de démarrage rapide et du fichier  [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=fr).
1. Créez une structure de dossiers sur votre ordinateur comme suit :

```plain
~/aem-sdk
    /author
    /publish
```

1. Renommez le fichier JAR [!DNL QuickStart] en ***aem-author-p4502.jar*** et placez-le dans le répertoire `/author`. Ajoutez le fichier ***[!DNL license.properties]*** sous le répertoire `/author`.

1. Copiez le fichier JAR [!DNL QuickStart], renommez-le en ***aem-publish-p4503.jar*** et placez-le dans le répertoire `/publish`. Copiez le fichier ***[!DNL license.properties]*** sous le répertoire `/publish`.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Double-cliquez sur le fichier ***aem-author-p4502.jar*** pour installer l’instance de **Création**. L’instance de création démarre et s’exécute sur le port **4502** de l’ordinateur local.

Double-cliquez sur le fichier ***aem-publish-p4503.jar*** pour installer l’instance de **Publication**. L’instance de publication démarre et s’exécute sur le port **4503** de l’ordinateur local.

>[!NOTE]
>
>Selon les ressources matérielles de votre machine de développement, il peut être difficile d’exécuter les instances de **Création et de publication** en même temps. Notez qu’il n’est pas courant d’exécuter les deux instances simultanément sur une configuration locale.

### Utiliser la ligne de commande

Au lieu de double-cliquer sur le fichier JAR, vous pouvez lancer AEM à partir de la ligne de commande ou créer un script (`.bat` ou `.sh`) en fonction de votre préférence de système d’exploitation local. Vous trouverez ci-dessous un exemple de commande :

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

Les `-X` sont des options JVM et les `-D` sont des propriétés de framework supplémentaires. Pour plus d’informations, consultez les articles [Déployer et maintenir une instance AEM](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=fr) et [Autres options disponibles dans le fichier de démarrage rapide](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html?lang=fr#further-options-available-from-the-quickstart-file).

## Installer Apache Maven

***[!DNL Apache Maven]*** est un outil permettant de gérer la procédure de création et de déploiement pour les projets Java. AEM est une plateforme Java et [!DNL Maven] est la méthode standard de gestion du code des projets AEM. Un ***Projet AEM Maven*** ou simplement un ***Projet AEM*** consiste en un projet Maven qui comprend tous le code *personnalisé* de votre site.

Tous les projets AEM doivent être créés à partir de la dernière version de l’**[!DNL AEM Project Archetype]** : [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). L’[!DNL AEM Project Archetype] fournit un bootstrap d’un projet AEM avec un exemple de code et de contenu. L’[!DNL AEM Project Archetype] inclut également des **[!DNL AEM WCM Core Components]** prêts à être utilisés dans votre projet.

>[!CAUTION]
>
>Lors du démarrage d’un nouveau projet, il est recommandé d’utiliser la dernière version de l’archétype. Gardez à l’esprit qu’il existe plusieurs versions de l’archétype et que toutes les versions ne sont pas compatibles avec les versions antérieures d’AEM.

### Étapes

1. Télécharger [Apache Maven](https://maven.apache.org/download.cgi)
2. Installez [Apache Maven](https://maven.apache.org/install.html) et vérifiez que l’installation a été ajoutée à votre ligne de commande `PATH`.
   * Les utilisateurs et utilisatrices de [!DNL macOS] peuvent installer Maven à l’aide de [Homebrew](https://brew.sh/).
3. Vérifiez que **[!DNL Maven]** est installé en ouvrant un nouveau terminal de ligne de commande et en exécutant la commande suivante :

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> Dans le passé, le profil Maven `adobe-public` était nécessaire pour que `nexus.adobe.com` télécharge les artefacts AEM. Tous les artefacts AEM sont désormais disponibles via Maven Central et le profil `adobe-public` n’est pas nécessaire.

## Configurer un environnement de développement intégré

Un environnement de développement intégré, ou IDE, est une application qui combine un éditeur de texte, la prise en charge de la syntaxe et des outils de création. Selon le type de développement entrepris, vous vous tournerez vers un IDE particulier. Quel que soit l’IDE, il est important de pouvoir ***déployer*** périodiquement le code sur une instance AEM locale afin de le tester. Il est important d’***intégrer*** de temps à autre les configurations d’une instance AEM locale dans votre projet AEM afin de les conserver dans un système de gestion de contrôle de code source, comme Git.

Vous trouverez ci-dessous quelques-uns des IDE les plus utilisés lors du développement AEM, accompagnés de vidéos qui montrent l’intégration à une instance AEM locale.

>[!NOTE]
>
> Le projet WKND a été mis à jour par défaut pour fonctionner sur AEM as a Cloud Service. Il a été mis à jour pour être [rétrocompatible avec les versions 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Si vous utilisez AEM 6.5 ou 6.4, ajoutez le profil `classic` à n’importe quelle commande Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Lorsque vous utilisez un IDE, vérifiez `classic` sous l’onglet Profil Maven.

![Onglet Profil Maven.](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Profil IntelliJ Maven*

### IDE [!DNL Eclipse]

L’**[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** est l’un des IDE les plus populaires pour le développement Java™, certainement parce qu’il est open source et ***gratuit***. Adobe fournit le plug-in **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=fr)** pour [!DNL Eclipse] afin de faciliter le développement dans une interface utilisateur graphique conviviale et de synchroniser le code avec une instance AEM locale. L’IDE [!DNL Eclipse] est recommandé aux développeurs et développeuses qui découvrent AEM, car les [!DNL AEM Developer Tools] prennent en charge son interface utilisateur graphique.

#### Installation et configuration

1. Téléchargez et installez l’IDE [!DNL Eclipse] pour [!DNL Java™ EE Developers] : [https://www.eclipse.org](https://www.eclipse.org/).
1. Suivez les instructions d’installation du plug-in [!DNL AEM Developer Tools] : [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=fr](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=fr).

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Import du projet Maven
* 01:24 - Création et déploiement du code source avec Maven
* 04:33 - Déploiement du nouveau code avec les outils de développement AEM
* 10:55 - Déploiement du nouveau code avec les outils de développement AEM
* 13:12 - Utilisation des outils de débogage intégrés d’Eclipse

### IntelliJ IDEA

**[IntelliJ IDEA](https://www.jetbrains.com/idea/)** est un IDE puissant pour le développement Java™ professionnel. [!DNL IntelliJ IDEA] est disponible en deux versions, l’édition ***gratuite*** [!DNL Community] et l’édition commerciale (payante) [!DNL Ultimate]. La version gratuite [!DNL Community] d’[!DNL IntellIJ IDEA] est suffisante pour le développement avec AEM, mais l’édition [!DNL Ultimate] [offre des fonctionnalités supplémentaires](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Téléchargez et installez [!DNL IntelliJ IDEA] : [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download).
1. Installez [!DNL Repo] (outil de ligne de commande) : [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Import du projet Maven
* 05:47 - Création et déploiement du code source avec Maven
* 08:17 - Envoi des modifications avec Repo
* 14:39 - Extraction des modifications avec Repo
* 17:25 - Utilisation des outils de débogage intégrés d’IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** a rapidement recueilli les faveurs des ***développeurs et développeuses front-end*** grâce à sa prise en charge améliorée de JavaScript, [!DNL Intellisense], ainsi que du débogage du navigateur. **[!DNL Visual Studio Code]** est open source, gratuit et doté de nombreuses extensions puissantes. [!DNL Visual Studio Code] peut être configuré pour s’intégrer à AEM à l’aide de l’outil Adobe, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Il existe également plusieurs extensions prises en charge par la communauté qui peuvent être installées pour s’intégrer à AEM.

[!DNL Visual Studio Code] est un excellent choix pour l’équipe de développement front-end qui écrit principalement du code CSS/LESS et JavaScript pour créer des bibliothèques clientes AEM. Cet outil n’est peut-être pas adapté aux nouveaux développeurs et développeuses AEM, car les définitions de nœud (boîtes de dialogue et composants) doivent être modifiées dans du code XML brut. Plusieurs extensions Java™ sont disponibles pour [!DNL Visual Studio Code], mais si vous faites principalement du développement Java™, [!DNL Eclipse IDE] ou [!DNL IntelliJ] peuvent constituer des options préférables.

#### Liens importants

* [**Télécharger**](https://code.visualstudio.com/Download) **Visual Studio Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** : outil de type FTP pour le contenu JCR.
* **[aemfed](https://aemfed.io)** : accélérer votre workflow front-end AEM.
* **[Synchroniser AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** : l’extension &#42; prise en charge par la communauté pour Visual Studio Code.

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Import du projet Maven
* 00:53 - Création et déploiement du code source avec Maven
* 04:03 - Envoi des modifications de code à l’aide de l’outil de ligne de commande Repo
* 08:29 - Extraction des modifications de code à l’aide de l’outil de ligne de commande Repo
* 10:40 - Envoi des modifications de code à l’aide de l’outil aemfed
* 14:24 - Dépannage et recréation de bibliothèques clientes

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html?lang=fr) est une vue navigateur du référentiel AEM. [!DNL CRXDE Lite] est incorporé dans AEM et permet à un développeur ou à une développeuse d’effectuer des tâches de développement standard telles que la modification de fichiers et la définition de composants, de boîtes de dialogue et de modèles. [!DNL CRXDE Lite] n’est ***pas*** conçu pour être un environnement de développement complet, mais il constitue un excellent outil de débogage. [!DNL CRXDE Lite] est utile pour étendre ou simplement comprendre le code de produit en dehors de votre base de code. [!DNL CRXDE Lite] fournit une vue puissante du référentiel et permet de tester et de gérer efficacement les autorisations.

[!DNL CRXDE Lite] doit être utilisé avec d’autres IDE pour tester et déboguer le code, mais jamais comme outil de développement principal. Il offre une prise en charge limitée de la syntaxe, ne dispose d’aucune fonctionnalité de saisie automatique et fournit une intégration limitée aux systèmes de gestion du contrôle de code source.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Résolution des problèmes

***À l’aide.*** Mon code ne fonctionne pas. Comme dans tout effort de développement, votre code peut ne pas fonctionner comme prévu. AEM est une plateforme puissante mais complexe. Vous trouverez ci-dessous les principales actions que vous pouvez entreprendre pour résoudre et suivre les problèmes (cette liste n’est en aucun cas exhaustive) :

### Vérifier le déploiement du code

En cas de problème, la première étape consiste à vérifier que le code a bien été déployé et installé sur AEM.

1. **Vérifiez le [!UICONTROL Gestionnaire de packages]** pour vous assurer que le package de code a été chargé et installé à l’adresse : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Vérifiez la date et l’heure pour vérifier que le package a été récemment installé.
1. Si vous effectuez des mises à jour incrémentielles de fichiers à l’aide d’un outil comme [!DNL Repo] ou [!DNL AEM Developer Tools], **vérifiez dans[!DNL CRXDE Lite]** que le fichier a été transmis à l’instance AEM locale et que son contenu est à jour : [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp).
1. **Vérifiez que le lot est chargé** si vous rencontrez des problèmes liés au code Java™ dans un lot OSGi. Ouvrez la [!UICONTROL console web d’Adobe Experience Manager] : [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) et recherchez votre lot. Assurez-vous que le statut du lot est défini sur **[!UICONTROL Actif]**. Consultez la section ci-dessous pour obtenir plus d’informations sur le dépannage d’un lot au statut **[!UICONTROL Installé]**.

#### Vérifier les journaux

AEM est une plateforme bavarde qui consigne les informations utiles dans le fichier **error.log**. Le fichier **error.log** se trouve dans le dossier d’installation d’AEM : &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Une technique utile pour effectuer le suivi des problèmes consiste à ajouter des instructions de journal dans votre code Java™ :

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

Par défaut, le fichier **error.log** est configuré pour consigner les insctructions *[!DNL INFO]*. Si vous souhaitez modifier le niveau de journal, accédez à [!UICONTROL Prise en charge des journaux] : [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Il se peut que vous trouviez le fichier **error.log** trop fourni. Utilisez la [!UICONTROL Prise en charge des journaux] pour configurer des instructions de journal pour un package Java™ spécifique. Il s’agit d’une bonne pratique pour les projets, afin de séparer facilement les problèmes de code personnalisé des problèmes concernant la plateforme AEM prête à l’emploi.

![Configuration de la journalisation dans AEM.](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Le lot est au statut Installé. {#bundle-active}

Tous les lots (à l’exception des fragments) doivent adopter le statut **[!UICONTROL Actif]**. Si votre lot de code est au statut [!UICONTROL Installé], un problème s’est produit. Il s’agit généralement d’un problème de dépendance :

![Erreur de lot dans AEM.](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Dans la copie d’écran ci-dessus, le statut du [!DNL WKND Core bundle] est [!UICONTROL Installé]. La raison est la suivante : le lot attend une version différente de `com.adobe.cq.wcm.core.components.models` qui est disponible sur l’instance AEM.

L’[!UICONTROL outil de recherche de dépendance Dependency Finder] peut s’avérer utile. Il est disponible à l’adresse suivante : [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Ajoutez le nom du package Java™ pour vérifier la version disponible sur l’instance AEM :

![Composants principaux.](assets/set-up-a-local-aem-development-environment/core-components.png)

Toujours dans l’exemple ci-dessus, nous constatons que la version installée sur l’instance AEM est **12.2**, alors que le lot attendait **12.6**. À partir de là, vous pouvez effectuer les étapes à l’envers et voir si les dépendances [!DNL Maven] sur AEM correspondent aux dépendances [!DNL Maven] du projet AEM. Dans l’exemple ci-dessus les [!DNL Core Components] **v2.2.0** sont installés sur l’instance AEM, mais le lot de code a été créé avec une dépendance sur **v2.2.2**, d’où la raison du problème de dépendance.

#### Vérifier l’enregistrement des modèles Sling {#osgi-component-sling-models}

Les composants AEM doivent être pris en charge par un [!DNL Sling Model] pour encapsuler toute logique commerciale et veiller à ce que le script de rendu HTL reste propre. Si le modèle Sling est introuvable, il peut s’avérer utile de vérifier les [!DNL Sling Models] dans la console : [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Cela vous indique si votre modèle Sling a été enregistré et à quel type de ressource (le chemin du composant) il est lié.

![Statut du modèle Sling.](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Affiche l’enregistrement d’un [!DNL Sling Model], `BylineImpl` lié à un type de ressource de composant de `wknd/components/content/byline`.

#### Problèmes CSS ou JavaScript

Pour la plupart des problèmes CSS et JavaScript, la méthode de dépannage la plus efficace est l’utilisation des outils de développement du navigateur. Pour réduire le problème lors du développement d’une instance de création AEM, il est utile d’afficher la page « telle que publiée ».

![Problèmes CSS ou JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Sélectionnez le menu [!UICONTROL Propriétés de la page], puis cliquez sur [!UICONTROL Afficher telle que publiée]. La page s’ouvre alors sans l’éditeur AEM et avec un paramètre de requête défini sur **wcmmode=disabled**. Cela désactive effectivement l’interface utilisateur de création AEM et facilite le dépannage/débogage des problèmes de front-end.

Un autre problème fréquent lors du développement du code front-end est un fichier CSS/JS ancien ou obsolète en cours de chargement. Dans un premier temps, assurez-vous que l’historique du navigateur a été effacé et démarrez éventuellement un navigateur en navigation privée ou une nouvelle session.

#### Déboguer des bibliothèques clientes

Avec les différentes méthodes de catégories et d’incorporations pour intégrer plusieurs bibliothèques clientes, la résolution des problèmes peut s’avérer fastidieuse. AEM propose plusieurs outils pour faciliter cette tâche. L’un des outils les plus intéressant est la [!UICONTROL Recréation des bibliothèques clientes] qui force AEM à recompiler les fichiers LESS et à générer le CSS.

* [Extraction des bibliothèques](http://localhost:4502/libs/granite/ui/content/dumplibs.html) : répertorie les bibliothèques clientes enregistrées dans l’instance AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Test de la sortie](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) : permet à un utilisateur ou une utilisatrice d’afficher la sortie HTML prévue des inclusions de bibliothèque cliente en fonction de la catégorie. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validation des dépendances des bibliothèques](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) : met en évidence les dépendances ou les catégories incorporées introuvables. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Recréation des bibliothèques clientes](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) : permet à un utilisateur ou une utilisatrice de forcer AEM à recréer les bibliothèques clientes ou d’invalider le cache des bibliothèques clientes. Cet outil est efficace lors du développement avec LESS, car il peut forcer AEM à recompiler le code CSS généré. En règle générale, il est plus efficace d’invalider les caches, puis d’actualiser la page plutôt que de recréer toutes les bibliothèques. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Débogage des bibliothèques clientes.](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si vous devez constamment invalider le cache à l’aide de l’outil [!UICONTROL Recréation des bibliothèques clientes], il peut être utile de recréer une seule fois toutes les bibliothèques clientes. Cela peut prendre environ 15 minutes, mais élimine généralement les futurs problèmes de mise en cache.
