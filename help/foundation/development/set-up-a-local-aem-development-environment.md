---
title: Configuration d’un environnement de développement AEM local
description: 'Découvrez comment configurer un environnement de développement local pour Experience Manager. Familiarisez-vous avec l’installation locale, Apache Maven, les environnements de développement intégrés, le débogage et la résolution des problèmes. Utilisez Eclipse IDE, CRXDE-Lite, Visual Studio Code et IntelliJ. '
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '2579'
ht-degree: 2%

---

# Configuration d’un environnement de développement AEM local

Guide de configuration d’un développement local pour Adobe Experience Manager, AEM. Couvre les rubriques importantes de l’installation locale, Apache Maven, les environnements de développement intégrés et le débogage/dépannage. Développement avec **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] et[!DNL IntelliJ]** sont discutées.

## Présentation

La configuration d’un environnement de développement local est la première étape du développement pour Adobe Experience Manager ou AEM. Prenez le temps de configurer un environnement de développement de qualité pour augmenter votre productivité et écrivez plus rapidement un meilleur code. Nous pouvons diviser un AEM environnement de développement local en 4 secteurs :

* Instances d’AEM locales
* [!DNL Apache Maven] project
* Environnements de développement intégrés (IDE)
* Résolution des problèmes

## Installation d’instances d’AEM locales

Lorsque nous nous référons à une instance d’AEM locale, nous parlons d’une copie d’Adobe Experience Manager en cours d’exécution sur la machine personnelle d’un développeur. ***Tous*** AEM développement doit commencer par écrire et exécuter du code sur une instance AEM locale.

Si vous êtes un utilisateur novice en AEM, deux modes d’exécution de base peuvent être installés : ***Auteur*** et ***Publier***. Le ***Auteur*** [runmode](https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  est l’environnement que les spécialistes du marketing numérique utiliseront pour créer et gérer du contenu. Lors du développement **most** de l’heure à laquelle vous déployez le code sur une instance d’auteur. Vous pouvez ainsi créer des pages, ajouter et configurer des composants. AEM Sites est un CMS de création WYSIWYG. La plupart des CSS et JavaScript peuvent donc être testés par rapport à une instance de création.

Il est également *critique* test du code par rapport à un élément local ***Publier*** instance. Le ***Publier*** est l’environnement AEM avec lequel les visiteurs de votre site web interagiront. Lorsque la variable ***Publier*** est la même pile technologique que l’objet ***Auteur*** il existe des différences majeures avec les configurations et les autorisations. Le code doit *always* être testé sur un site local ***Publier*** avant d’être convertie en environnements de niveau supérieur.

### Étapes

1. Assurez-vous que [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) est installé.
   * Préférence [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=14) pour AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) pour les versions AEM antérieures à AEM 6.5
2. Obtention d’une copie de la variable [AEM QuickStart Jar et un [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Créez une structure de dossiers sur votre ordinateur comme suit :

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Renommez la variable [!DNL QuickStart] JAR à ***aem-author-p4502.jar*** et placez-le sous le `/author` répertoire . Ajoutez la variable ***[!DNL license.properties]*** sous le fichier `/author` répertoire .
5. Effectuez une copie de la fonction [!DNL QuickStart] JAR, renommez-le ***aem-publish-p4503.jar*** et placez-le sous le `/publish` répertoire . Ajoutez une copie de la fonction ***[!DNL license.properties]*** sous le fichier `/publish` répertoire .

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Double-cliquez sur le ***aem-author-p4502.jar*** pour installer le fichier **Auteur** instance. Cette opération démarre l’instance d’auteur en cours d’exécution sur le port . **4502** sur l&#39;ordinateur local.

   Double-cliquez sur le ***aem-publish-p4503.jar*** pour installer le fichier **Publier** instance. L’instance de publication démarre alors sur le port . **4503** sur l&#39;ordinateur local.

   >[!NOTE]
   >
   >Selon le matériel de votre machine de développement, il peut être difficile d’avoir à la fois une **Création et publication** s’exécutant en même temps. Vous devez rarement exécuter les deux simultanément sur une configuration locale.

   Pour plus d’informations, voir [Déploiement et maintenance d’une instance AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Installer Apache Maven

***[!DNL Apache Maven]*** est un outil permettant de gérer la procédure de création et de déploiement pour les projets Java. AEM est une plateforme Java et [!DNL Maven] est la méthode standard de gestion du code pour un projet AEM. Quand nous disons ***Projet AEM Maven*** ou simplement votre ***AEM projet***, nous faisons référence à un projet Maven qui comprend tous les *custom* du code de votre site.

Tous les projets AEM doivent être créés à partir de la dernière version de la variable **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). Le [!DNL AEM Project Archetype] crée un bootstrap d’un projet AEM avec un exemple de code et de contenu. Le [!DNL AEM Project Archetype] inclut également **[!DNL AEM WCM Core Components]** configuré pour être utilisé sur votre projet.

>[!CAUTION]
>
>Lors du démarrage d’un nouveau projet, il est recommandé d’utiliser la dernière version de l’archétype. Gardez à l’esprit qu’il existe plusieurs versions de l’archétype et que toutes les versions ne sont pas compatibles avec les versions antérieures d’AEM.

### Étapes

1. Télécharger [Apache Maven](https://maven.apache.org/download.cgi)
2. Installer [Apache Maven](https://maven.apache.org/install.html) et assurez-vous que l’installation a été ajoutée à votre ligne de commande. `PATH`.
   * [!DNL macOS] Les utilisateurs peuvent installer Maven à l’aide de [Homebrew](https://brew.sh/)
3. Vérifiez que **[!DNL Maven]** est installé en ouvrant un nouveau terminal de ligne de commande et en exécutant ce qui suit :

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
   > Dans le passé, l’ajout de `adobe-public` Le profil Maven était nécessaire pour pointer `nexus.adobe.com` pour télécharger AEM artefacts. Tous les artefacts AEM sont désormais disponibles via Maven Central et le `adobe-public` profile n’est pas nécessaire.

## Configuration d’un environnement de développement intégré

Un environnement de développement intégré ou IDE est une application qui combine un éditeur de texte, la prise en charge de la syntaxe et des outils de création. Selon le type de développement que vous effectuez, un IDE peut être préférable à un autre. Quel que soit l’IDE, il sera important de pouvoir périodiquement ***push*** code vers une instance d’AEM locale afin de la tester. Il sera également important de ***pull*** configurations d’une instance d’AEM locale dans votre projet AEM afin de persister dans un système de gestion de contrôle de code source comme Git.

Vous trouverez ci-dessous quelques-uns des IDE les plus utilisés avec le développement AEM avec les vidéos correspondantes qui montrent l’intégration à une instance AEM locale.

>[!NOTE]
>
> Le projet WKND a été mis à jour par défaut pour fonctionner sur AEM as a Cloud Service. Il a été mis à jour pour être [rétrocompatible avec la version 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Si vous utilisez AEM version 6.5 ou 6.4, ajoutez la variable `classic` profile à n’importe quelle commande Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Lorsque vous utilisez un IDE, veillez à vérifier `classic` dans l’onglet Profil Maven .

![Onglet Profil Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Profil IntelliJ Maven*

### [!DNL Eclipse] IDE

Le **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** est l’un des IDE les plus populaires pour le développement de Java, en grande partie parce qu’il est open source et ***free***! Adobe fournit un module externe, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)**, pour [!DNL Eclipse] pour faciliter le développement avec une interface utilisateur graphique conviviale afin de synchroniser le code avec une instance d’AEM locale. Le [!DNL Eclipse] Il est recommandé aux développeurs qui découvrent l’AEM en grande partie en raison de la prise en charge de l’interface utilisateur graphique par [!DNL AEM Developer Tools].

#### Installation et configuration

1. Téléchargez et installez le [!DNL Eclipse] IDE pour [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Suivez les instructions d’installation du [!DNL AEM Developer Tools] module externe : [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importer un projet Maven
* 01:24 - Création et déploiement du code source avec Maven
* 04:33 - Modifications du code push avec AEM Developer Tool
* 10:55 - Extraction de modifications de code avec l’outil de développement AEM
* 13:12 - Utilisation des outils de débogage intégrés d’Eclipse

### IntelliJ IDEA

Le **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** est un IDE puissant pour le développement professionnel de Java. [!DNL IntelliJ IDEA] vient en deux versions, a ***free*** [!DNL Community] édition et une publicité (payante) [!DNL Ultimate] version. Le libre [!DNL Community] version de [!DNL IntellIJ IDEA] est suffisant pour AEM développement, mais la variable [!DNL Ultimate] [développe son ensemble de fonctionnalités.](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Téléchargez et installez le [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Installer [!DNL Repo] (outil de ligne de commande) : [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importer un projet Maven
* 05:47 - Création et déploiement du code source avec Maven
* 08:17 - Push changes with Repo
* 14:39 - Extraction des modifications avec Repo
* 17:25 - Utilisation des outils de débogage intégrés d’IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Visual Studio Code](https://code.visualstudio.com/)** est rapidement devenu un outil favori pour ***développeurs front-end*** avec une prise en charge améliorée de JavaScript, [!DNL Intellisense]et prise en charge du débogage du navigateur. **[!DNL Visual Studio Code]** est open source, gratuit, avec de nombreuses extensions puissantes. [!DNL Visual Studio Code] peut être configuré pour s’intégrer à AEM à l’aide d’un outil Adobe, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Il existe également plusieurs extensions prises en charge par la communauté qui peuvent être installées pour s’intégrer à AEM.

[!DNL Visual Studio Code] est un excellent choix pour les développeurs front-end qui écriront principalement du code CSS/LESS et JavaScript pour créer AEM bibliothèques clientes. Cet outil n’est peut-être pas le meilleur choix pour les nouveaux développeurs d’AEM, car les définitions de noeud (boîtes de dialogue, composants) devront toutes être modifiées dans du code XML brut. Plusieurs extensions Java sont disponibles pour [!DNL Visual Studio Code], mais principalement en cas de développement Java [!DNL Eclipse IDE] ou [!DNL IntelliJ] peut être préférable.

#### Liens importants

* [**Télécharger**](https://code.visualstudio.com/Download) **Visual Studio Code**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Outil de type FTP pour le contenu JCR
* **[aemfed](https://aemfed.io/)** - Accélérer votre workflow front-end AEM
* **[Synchronisation des AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Prise en charge par la communauté&#42; extension pour Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importer un projet Maven
* 00:53 - Création et déploiement du code source avec Maven
* 04:03 - Modifications du code push à l’aide de l’outil de ligne de commande Repo
* 08:29 - Extraction des modifications de code à l’aide de l’outil de ligne de commande Repo
* 10:40 - Modifications du code push avec l’outil aemfed
* 14:24 - Dépannage, reconstruction des bibliothèques clientes

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) est une vue navigateur du référentiel AEM. [!DNL CRXDE Lite] est incorporé dans AEM et permet à un développeur d’effectuer des tâches de développement standard telles que la modification de fichiers, la définition de composants, de boîtes de dialogue et de modèles. [!DNL CRXDE Lite] is ***not*** est conçu pour être un environnement de développement complet, mais est très efficace en tant qu’outil de débogage. [!DNL CRXDE Lite] est utile pour étendre ou simplement comprendre le code de produit en dehors de votre base de code. [!DNL CRXDE Lite] fournit une vue puissante du référentiel et un moyen de tester et de gérer efficacement les autorisations.

[!DNL CRXDE Lite] doit toujours être utilisé conjointement avec d’autres IDE pour tester et déboguer le code, mais jamais comme outil de développement Principal. Il offre une prise en charge de la syntaxe limitée, aucune fonctionnalité de saisie automatique et une intégration limitée aux systèmes de gestion du contrôle de code source.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Résolution des problèmes

***Aide!*** Mon code ne fonctionne pas ! Comme pour tout développement, il y aura des moments (probablement plusieurs) où votre code ne fonctionne simplement pas comme prévu. AEM est une plate-forme puissante, mais avec une grande puissance... vient une grande complexité. Vous trouverez ci-dessous quelques points de départ de haut niveau concernant le dépannage et le suivi des problèmes (mais loin d’être une liste exhaustive des problèmes qui peuvent se produire) :

### Vérification du déploiement du code

Une bonne première étape consiste, en cas de problème, à vérifier que le code a bien été déployé et installé sur AEM.

1. **Vérifier [!UICONTROL Gestionnaire de modules]** pour vous assurer que le package de code a été téléchargé et installé : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Vérifiez l’horodatage pour vérifier que le package a été récemment installé.
1. Si vous effectuez des mises à jour de fichier incrémentielles à l’aide d’un outil comme [!DNL Repo] ou [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** que le fichier a été transmis à l’instance d’AEM locale et que le contenu du fichier est mis à jour : [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Vérifiez que le lot est téléchargé.** si vous rencontrez des problèmes liés au code Java dans un lot OSGi. Ouvrez le [!UICONTROL Console web Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) et recherchez votre lot. Assurez-vous que le lot a une **[!UICONTROL Principal]** statut. Voir ci-dessous pour plus d’informations sur le dépannage d’un lot dans une **[!UICONTROL Installé]** état.

#### Vérification des journaux

AEM est une plate-forme de discussion et consigne de nombreuses informations utiles dans le journal **error.log**. Le **error.log** est disponible là où AEM a été installé : &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Une technique utile pour effectuer le suivi des problèmes consiste à ajouter des instructions de journal dans votre code Java :

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

Par défaut, la variable **error.log** est configuré pour le journal *[!DNL INFO]* des instructions. Si vous souhaitez modifier le niveau de journal, vous pouvez le faire en accédant à [!UICONTROL Prise en charge des journaux]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Il se peut également que la variable **error.log** est trop bavard. Vous pouvez utiliser la variable [!UICONTROL Prise en charge des journaux] pour configurer les instructions de journal pour un package Java spécifié uniquement. Il s’agit d’une bonne pratique pour les projets, afin de séparer facilement les problèmes de code personnalisé des problèmes de plateforme AEM prêts à l’emploi.

![Configuration de journalisation dans AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### Le lot est à l’état Installé {#bundle-active}

Tous les lots (à l’exception des fragments) doivent se trouver dans une **[!UICONTROL Principal]** état. Si votre lot de code s’affiche dans une [!UICONTROL Installé] il existe alors un problème qui doit être résolu. Il s’agit généralement d’un problème de dépendance :

![Erreur de bundle dans AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Dans la capture d’écran ci-dessus, la fonction [!DNL WKND Core bundle] est un [!UICONTROL Installé] état. En effet, le lot attend une version différente de `com.adobe.cq.wcm.core.components.models` qui est disponible sur l’instance AEM.

Un outil utile qui peut être utilisé est le suivant : [!UICONTROL Outil de recherche des dépendances]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Ajoutez le nom du package Java pour vérifier quelle version est disponible sur l’instance AEM :

![Composants principaux](assets/set-up-a-local-aem-development-environment/core-components.png)

En reprenant l’exemple ci-dessus, nous pouvons constater que la version installée sur l’instance AEM est **12.2** vs **12,6** que le lot attendait. À partir de là, vous pouvez travailler à l’envers et voir si la variable [!DNL Maven] les dépendances sur AEM correspondent à la variable [!DNL Maven] dépendances du projet AEM. Dans l’exemple ci-dessus [!DNL Core Components] **v2.2.0** est installé sur l’instance AEM, mais le lot de code a été créé avec une dépendance sur **v2.2.2**, d’où la raison du problème de dépendance.

#### Vérification de l’enregistrement des modèles Sling {#osgi-component-sling-models}

AEM composants doivent toujours être pris en charge par une [!DNL Sling Model] pour encapsuler toute logique commerciale et veiller à ce que le script de rendu HTL reste propre. Si vous rencontrez des problèmes où le modèle Sling est introuvable, il peut s’avérer utile de vérifier la variable [!DNL Sling Models] à partir de la console : [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Cela vous indique si votre modèle Sling a été enregistré et à quel type de ressource (le chemin d’accès au composant) il est lié.

![État du modèle Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Affiche l’enregistrement d’une [!DNL Sling Model], `BylineImpl` qui est lié à un type de ressource de composant de `wknd/components/content/byline`.

#### Problèmes CSS ou JavaScript

Pour la plupart des problèmes CSS et JavaScript, l’utilisation des outils de développement du navigateur est la méthode de dépannage la plus efficace. Pour réduire le problème lors du développement par rapport à une instance d’auteur AEM, il est utile d’afficher la page &quot;en tant que publiée&quot;.

![Problèmes CSS ou JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Ouvrez le [!UICONTROL Propriétés de la page] et cliquez sur [!UICONTROL Afficher comme publié(e)]. La page s’ouvre alors sans l’éditeur d’AEM et avec un paramètre de requête défini sur **wcmmode=disabled**. Cela permet de désactiver efficacement l’interface utilisateur de création d’AEM et de faciliter le dépannage/débogage des problèmes front-end.

Un autre problème fréquemment rencontré lors du développement du code frontal est un fichier CSS/JS ancien ou obsolète en cours de chargement. Dans un premier temps, assurez-vous que l’historique du navigateur a été effacé et, au besoin, lancez un navigateur incognito ou une nouvelle session.

#### Débogage des bibliothèques clientes

Avec différentes méthodes de catégories et d’incorporations pour inclure plusieurs bibliothèques clientes, le dépannage peut s’avérer fastidieux. AEM expose plusieurs outils pour faciliter cette tâche. L’un des outils les plus importants est [!UICONTROL Reconstruire les bibliothèques clientes] qui force AEM à recompiler les fichiers LESS et à générer le CSS.

* [Effacer les bibliothèques](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Répertorie toutes les bibliothèques clientes enregistrées dans l’instance AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Test Output](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permet à un utilisateur d’afficher la sortie de HTML attendue des inclusions clientlib en fonction de la catégorie. &lt;host>/libs/granite/ui/content/dumplibs.test
* [Validation des dépendances des bibliothèques](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - met en évidence les dépendances ou les catégories incorporées introuvables. &lt;host>/libs/granite/ui/content/dumplibs.validate
* [Reconstruire les bibliothèques clientes](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permet à un utilisateur de forcer AEM à recréer toutes les bibliothèques clientes ou d’invalider le cache des bibliothèques clientes. Cet outil est particulièrement efficace lors du développement avec LESS, car cela peut forcer AEM à recompiler le CSS généré. En règle générale, il est plus efficace d’invalider les caches, puis d’effectuer une actualisation de page plutôt que de recréer toutes les bibliothèques. &lt;host>/libs/granite/ui/content/dumplibs.rebuild

![Débogage des bibliothèques clientes](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Si vous devez constamment invalider le cache à l’aide de la variable [!UICONTROL Reconstruire les bibliothèques clientes] pour effectuer une reconstruction unique de toutes les bibliothèques clientes. Cela peut prendre environ 15 minutes, mais élimine généralement les problèmes de mise en cache à l’avenir.
