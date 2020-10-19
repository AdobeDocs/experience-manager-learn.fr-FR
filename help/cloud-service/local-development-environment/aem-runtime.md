---
title: Configuration de l’exécution des AEM locales pour l’AEM en tant que développement Cloud Service
description: Configurez l’AEM d’exécution locale à l’aide de l’AEM en tant que Jar de démarrage rapide du kit SDK Cloud Service.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: 4cfbf975919eb38413be8446b70b107bbfebb845
workflow-type: tm+mt
source-wordcount: '1406'
ht-degree: 1%

---


# Configuration de l’AEM d’exécution locale

Adobe Experience Manager (AEM) peut être exécuté localement à l’aide de l’AEM en tant que Jar de démarrage rapide du Cloud Service SDK. Cela permet aux développeurs de se déployer et de tester du code, des configurations et du contenu personnalisés avant de s&#39;engager dans le contrôle de code source et de le déployer sur un AEM en tant qu&#39;environnement Cloud Service.

Notez qu’ `~` il est utilisé comme abrégé pour l’annuaire d’utilisateurs. Sous Windows, c&#39;est l&#39;équivalent de `%HOMEPATH%`.

## Installer Java

Le Experience Manager est une application Java et nécessite donc le SDK Java pour prendre en charge l’outil de développement.

1. [Téléchargement et installation de la dernière version du SDK Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=liste&amp;p.offset=0&amp;p.limit=14)
1. Vérifiez que le SDK Java 11 est installé en exécutant la commande :
   + Windows : `java -version`
   + macOS / Linux : `java --version`

![Java](./assets/aem-runtime/java.png)

## Téléchargement de l’AEM en tant que SDK Cloud Service

L’AEM en tant que Cloud Service SDK, ou AEM SDK, contient le Jar de démarrage rapide utilisé pour exécuter AEM Author et Publish localement pour le développement, ainsi que la version compatible des outils du répartiteur.

1. Log in to [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) with your Adobe ID
   + Notez que votre organisation d’Adobes __doit__ être configurée pour AEM en tant que Cloud Service pour télécharger l’AEM en tant que SDK Cloud Service.
1. Accédez à l’ __AEM en tant que Cloud Service__ .
1. Trier par date __de__ publication dans l’ordre __Descendant__
1. Cliquez sur la dernière ligne __AEM résultat SDK__ .
1. Examinez et acceptez le contrat de licence de l’utilisateur final, puis appuyez sur le bouton __Télécharger__ .

## Extrayez le fichier Jar de démarrage rapide du fichier compressé AEM SDK

1. Unzip the downloaded `aem-sdk-XXX.zip` file

## Configuration du service AEM Author local{#set-up-local-aem-author-service}

Le service local d’auteur AEM offre aux développeurs une expérience locale que les spécialistes du marketing numérique et les auteurs de contenu partageront pour créer et gérer du contenu.  Le service Auteur AEM est conçu à la fois comme un environnement de création et d’prévisualisation, ce qui permet d’effectuer la plupart des validations du développement de fonctionnalités par rapport à celui-ci, ce qui en fait un élément essentiel du processus de développement local.

1. Create the folder `~/aem-sdk/author`
1. Copiez le fichier JAR ____ Quickstart dans `~/aem-sdk/author` et renommez-le en `aem-author-p4502.jar`
1. Début du service d’auteur AEM local en exécutant les éléments suivants à partir de la ligne de commande :
   + `java -jar aem-author-p4502.jar`
      + Fournissez le mot de passe d’administrateur sous la forme `admin`. Tout mot de passe d’administrateur est acceptable, mais il est recommandé d’utiliser la valeur par défaut pour le développement local afin de réduire la nécessité de reconfigurer.

   Vous *ne pouvez pas* début l&#39;AEM en tant que Jar de démarrage rapide Cloud Service [en cliquant sur](#troubleshooting-double-click)un doublon.
1. Accédez au service AEM Author local à l’adresse [http://localhost:4502](http://localhost:4502) dans un navigateur Web.

Windows :

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux :

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Configuration du service local de publication AEM

Le service de publication AEM local fournit aux développeurs l’expérience locale que les utilisateurs finaux de l’AEM auront, comme la navigation sur le site Web hébergé sur AEM. Un service de publication AEM local est important car il s’intègre aux outils [](./dispatcher-tools.md) Répartiteur du SDK AEM et permet aux développeurs de tester la fumée et d’affiner l’expérience de l’utilisateur final face à l’utilisateur final.

1. Create the folder `~/aem-sdk/publish`
1. Copiez le fichier JAR ____ Quickstart dans `~/aem-sdk/publish` et renommez-le en `aem-publish-p4503.jar`
1. Début du service de publication AEM local en exécutant les éléments suivants à partir de la ligne de commande :
   + `java -jar aem-publish-p4503.jar`
      + Fournissez le mot de passe d’administrateur sous la forme `admin`. Tout mot de passe d’administrateur est acceptable, mais il est recommandé d’utiliser la valeur par défaut pour le développement local afin de réduire la nécessité de reconfigurer.

   Vous *ne pouvez pas* début l&#39;AEM en tant que Jar de démarrage rapide Cloud Service [en cliquant sur](#troubleshooting-double-click)un doublon.
1. Accédez au service de publication AEM local à l’adresse [http://localhost:4503](http://localhost:4503) dans un navigateur Web.

Windows :

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux :

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Modes de démarrage rapide des débuts JAR

Le nom de la Jar de démarrage rapide `aem-<tier>_<environment>-p<port number>.jar` indique comment elle va se début. Une fois AEM démarré dans un niveau, un auteur ou une publication spécifique, il ne peut plus être remplacé par un autre niveau. Pour ce faire, le `crx-Quickstart` dossier généré lors de la première exécution doit être supprimé et le fichier Jar de démarrage rapide doit être réexécuté. L&#39;Environnement et les ports peuvent être modifiés, mais ils nécessitent un arrêt/début de l&#39;instance d&#39;AEM locale.

La modification des environnements `dev``stage` et `prod`peut être utile aux développeurs pour s’assurer que les configurations spécifiques à un environnement sont correctement définies et résolues par AEM. Il est recommandé que le développement local soit principalement effectué par rapport au mode d’exécution par `dev` environnement par défaut.

Les permutations disponibles sont les suivantes :

+ `aem-author-p4502.jar`
   + En tant qu’auteur en mode d’exécution Dev sur le port 4502
+ `aem-author_dev-p4502.jar`
   + Auteur en mode d’exécution Dev sur le port 4502 (identique à `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + En tant qu’auteur en mode d’exécution intermédiaire sur le port 4502
+ `aem-author_prod-p4502.jar`
   + En tant qu’auteur en mode d’exécution Production sur le port 4502
+ `aem-publish-p4503.jar`
   + En tant qu’auteur en mode d’exécution Dev sur le port 4503
+ `aem-publish_dev-p4503.jar`
   + Auteur en mode d’exécution Dev sur le port 4503 (identique à `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + En tant qu’auteur en mode d’exécution intermédiaire sur le port 4503
+ `aem-publish_prod-p4503.jar`
   + En tant qu’auteur en mode d’exécution Production sur le port 4503

Notez que le numéro de port peut être n’importe quel port disponible sur l’ordinateur de développement local, mais par convention :

+ Le port __4502__ est utilisé pour le service __local AEM Author.__
+ Le port __4503__ est utilisé pour le service de publication AEM __local.__

La modification de ces paramètres peut nécessiter des ajustements aux configurations AEM SDK

## Arrêt d’un AEM d’exécution local

Pour arrêter un AEM d’exécution local, qu’il s’agisse d’AEM Author ou du service de publication, ouvrez la fenêtre de ligne de commande qui a été utilisée pour début de l’exécution AEM, puis appuyez sur `Ctrl-C`. Attendez que AEM se ferme. Une fois le processus d&#39;arrêt terminé, l&#39;invite de ligne de commande est disponible.

## Quand mettre à jour le fichier JAR de démarrage rapide

Mettez à jour le SDK AEM au moins une fois par mois, ou peu après, le dernier jeudi de chaque mois, qui est la cadence de publication pour AEM en tant que &quot;versions de fonctionnalités&quot; Cloud Service.

>[!WARNING]
>
> La mise à jour du fichier Jar de démarrage rapide vers une nouvelle version nécessite le remplacement de l’ensemble de l’environnement de développement local, ce qui entraîne la perte de tout le code, la configuration et le contenu dans les référentiels d’AEM locaux. Assurez-vous que tout code, configuration ou contenu qui ne doit pas être détruit est valorisé en toute sécurité dans Git ou exporté depuis l’instance AEM locale en tant que paquets AEM.

### Comment éviter la perte de contenu lors de la mise à niveau du SDK AEM

La mise à niveau du SDK AEM crée effectivement un nouveau runtime AEM, y compris un nouveau référentiel, ce qui signifie que toute modification apportée à un référentiel de SDK précédent est perdue. Les stratégies suivantes sont viables pour aider à conserver le contenu entre les mises à niveau AEM SDK et peuvent être utilisées séparément ou de concert :

1. Créez un package de contenu dédié à contenir un &quot;exemple&quot; de contenu pour faciliter le développement, et conservez-le dans Git. Tout contenu qui doit être conservé par le biais des mises à niveau AEM SDK sera conservé dans ce pack et redéployé après la mise à niveau du SDK AEM.
1. Utilisez [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) avec la `includepaths` directive, pour copier du contenu du référentiel SDK AEM précédent vers le nouveau référentiel SDK AEM.
1. Sauvegardez tout contenu à l’aide AEM Package Manager et des packages de contenu sur le SDK AEM précédent, puis réinstallez-les sur le nouveau SDK AEM.

N’oubliez pas que l’utilisation des approches ci-dessus pour gérer le code entre les mises à niveau AEM SDK indique un modèle de développement anti-schéma. Le code non jetable doit provenir de votre IDE de développement et être acheminé vers AEM SDK via des déploiements.

## Résolution des incidents

## Un doublon-clic sur le fichier JAR de démarrage rapide entraîne une erreur.{#troubleshooting-double-click}

Lorsque vous cliquez sur le doublon Quickstart Jar to début, une erreur modale s’affiche pour empêcher AEM de démarrer localement.

![Dépannage - Doublon-clic sur le fichier JAR de démarrage rapide](./assets/aem-runtime/troubleshooting__double-click.png)

Cela est dû au fait que AEM en tant que Jar de démarrage rapide Cloud Service ne prend pas en charge les clics doublons du Jar de démarrage rapide pour début AEM localement. Au lieu de cela, vous devez exécuter le fichier JAR à partir de cette ligne de commande.

Pour début du service Auteur AEM, `cd` dans le répertoire contenant le fichier Jar de démarrage rapide et exécutez la commande :

`$ java -jar aem-author-p4502.jar`

ou, pour début au service AEM Publish, `cd` dans le répertoire contenant le fichier Jar de démarrage rapide et exécutez la commande :

`$ java -jar aem-author-p4503.jar`

## Le démarrage du fichier JAR de démarrage rapide à partir de la ligne de commande interrompt immédiatement{#troubleshooting-java-8}

Lors du démarrage du fichier Jar de démarrage rapide à partir de la ligne de commande, le processus s’interrompt immédiatement et le service AEM ne s’début pas, avec l’erreur suivante :

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

La raison en est que AEM en tant que Cloud Service requiert Java SDK 11 et que vous exécutez une autre version, probablement Java 8. Pour résoudre ce problème, téléchargez et installez [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=liste&amp;p.offset=0&amp;p.limit=14).
Une fois Java SDK 11 installé, vérifiez qu’il s’agit de la version principale en exécutant les éléments suivants à partir de la ligne de commande.

Une fois le SDK Java 11 installé, vérifiez qu’il s’agit de la version principale en exécutant la commande à partir de la ligne de commande :

+ Windows : `java -version`
+ macOS / Linux : `java --version`

## Ressources supplémentaires

+ [Télécharger AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Télécharger le Docteur](https://www.docker.com/)
+ [Documentation du répartiteur Experience Manager](https://docs.adobe.com/content/help/fr-FR/experience-manager-dispatcher/using/dispatcher.html)
