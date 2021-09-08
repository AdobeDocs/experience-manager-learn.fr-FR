---
title: Configuration de l’exécution AEM locale pour AEM en tant que développement de Cloud Service
description: Configurez le Runtime AEM local à l’aide du fichier Jar de démarrage rapide du SDK AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 3%

---


# Configuration de l’exécution d’AEM locale

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Exécution locale AEM"
>abstract="Adobe Experience Manager (AEM) peut être exécuté localement à l’aide de l’AEM as a Cloud Service Quickstart Jar du SDK. Cela permet aux développeurs de déployer et de tester du code personnalisé, des configurations et du contenu avant de le soumettre au contrôle de code source et de le déployer dans un environnement AEM en tant qu’environnement de Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="SDK AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Téléchargement d’AEM en tant que SDK Cloud Service"

Adobe Experience Manager (AEM) peut être exécuté localement à l’aide de l’AEM as a Cloud Service Quickstart Jar du SDK. Cela permet aux développeurs de déployer et de tester du code personnalisé, des configurations et du contenu avant de le soumettre au contrôle de code source et de le déployer dans un environnement AEM en tant qu’environnement de Cloud Service.

Notez que `~` est utilisé comme abrégé pour le répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

## Installer Java

Experience Manager est une application Java qui requiert donc le SDK Java pour prendre en charge l’outil de développement.

1. [Télécharger et installer le dernier SDK Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. Vérifiez que le SDK Java 11 est installé en exécutant la commande :
   + Windows : `java -version`
   + macOS / Linux : `java --version`

![Java](./assets/aem-runtime/java.png)

## Téléchargement de l’AEM en tant que SDK Cloud Service

Le SDK AEM as a Cloud Service, ou SDK AEM, contient le fichier Quickstart Jar utilisé pour exécuter AEM Author et Publier localement pour le développement, ainsi que la version compatible des outils Dispatcher.

1. Connectez-vous à [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) avec votre Adobe ID.
   + Notez que votre organisation d’Adobe __doit__ être configurée pour AEM en tant que Cloud Service pour télécharger AEM en tant que SDK de Cloud Service.
1. Accédez à l’onglet __AEM en tant que Cloud Service__
1. Tri par __Date de publication__ dans l’ordre __Décroissance__
1. Cliquez sur la dernière __ligne de résultat AEM SDK__
1. Vérifiez et acceptez le CLUF, puis appuyez sur le bouton __Télécharger__

## Extrayez le fichier Jar de démarrage rapide du fichier zip du SDK AEM

1. Décompressez le fichier `aem-sdk-XXX.zip` téléchargé.

## Configuration du service AEM Author local{#set-up-local-aem-author-service}

Le service AEM Author local fournit aux développeurs une expérience locale que les spécialistes du marketing numérique/auteurs de contenu peuvent partager pour créer et gérer du contenu.  Le service d’auteur AEM est conçu comme un environnement de création et de prévisualisation. Il permet d’effectuer la plupart des validations du développement de fonctionnalités par rapport à celui-ci, ce qui en fait un élément essentiel du processus de développement local.

1. Créez le dossier `~/aem-sdk/author`
1. Copiez le fichier __JAR de démarrage rapide__ dans `~/aem-sdk/author` et renommez-le `aem-author-p4502.jar`.
1. Démarrez le service AEM Author local en exécutant les éléments suivants à partir de la ligne de commande :
   + `java -jar aem-author-p4502.jar`
      + Saisissez le mot de passe administrateur `admin`. Tout mot de passe administrateur est acceptable, mais il est recommandé d’utiliser la valeur par défaut pour le développement local afin de réduire la nécessité de reconfigurer.

   Vous *ne pouvez pas* démarrer l’AEM comme Jar de démarrage rapide de Cloud Service [en double-cliquant sur ](#troubleshooting-double-click).
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

## Configuration du service de publication AEM local

Le service de publication AEM local fournit aux développeurs l’expérience locale que les utilisateurs finaux de l’AEM auront, comme la navigation sur le site Web hébergé sur AEM. Un service de publication AEM local est important, car il s’intègre aux [outils Dispatcher du SDK d’AEM et permet aux développeurs de tester la fumée et d’affiner l’expérience de contact de l’utilisateur final.](./dispatcher-tools.md)

1. Créez le dossier `~/aem-sdk/publish`
1. Copiez le fichier __JAR de démarrage rapide__ dans `~/aem-sdk/publish` et renommez-le `aem-publish-p4503.jar`.
1. Démarrez le service de publication AEM local en exécutant les éléments suivants à partir de la ligne de commande :
   + `java -jar aem-publish-p4503.jar`
      + Saisissez le mot de passe administrateur `admin`. Tout mot de passe administrateur est acceptable, mais il est recommandé d’utiliser la valeur par défaut pour le développement local afin de réduire la nécessité de reconfigurer.

   Vous *ne pouvez pas* démarrer l’AEM comme Jar de démarrage rapide de Cloud Service [en double-cliquant sur ](#troubleshooting-double-click).
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

## Simulation de la distribution de contenu {#content-distribution}

Dans un environnement de Cloud Service réel, le contenu est distribué du service de création au service de publication à l’aide de [la distribution de contenu Sling](https://sling.apache.org/documentation/bundles/content-distribution.html) et du pipeline d’Adobe. Le [pipeline d’Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) est un microservice isolé disponible uniquement dans l’environnement cloud.

Pendant le développement, il peut être souhaitable de simuler la distribution du contenu à l’aide du service local Auteur et Publication. Pour ce faire, activez les agents de réplication hérités.

>[!NOTE]
>
> Les agents de réplication ne peuvent être utilisés que dans le fichier JAR Quickstart local et ne fournissent qu’une simulation de la distribution de contenu.

1. Connectez-vous au service **Auteur** et accédez à [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Cliquez sur **Agent par défaut (publication)** pour ouvrir l’agent de réplication par défaut.
1. Cliquez sur **Modifier** pour ouvrir la configuration de l’agent.
1. Sous l’onglet **Paramètres** , mettez à jour les champs suivants :

   + **Activé**  - cochez la valeur true
   + **ID utilisateur de l’agent**  : laissez ce champ vide

   ![Configuration de l’agent de réplication - Paramètres](assets/aem-runtime/settings-config.png)

1. Sous l’onglet **Transport** , mettez à jour les champs suivants :

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **User** - `admin`
   + **Mot de passe** - `admin`

   ![Configuration de l’agent de réplication - Transport](assets/aem-runtime/transport-config.png)

1. Cliquez sur **Ok** pour enregistrer la configuration et activer l’agent de réplication **Par défaut**.
1. Vous pouvez maintenant apporter des modifications au contenu sur le service Auteur et les publier sur le service Publication.

![Publier la page](assets/aem-runtime/publish-page-changes.png)

## Modes de démarrage rapide de Jar

Le nom du fichier Quickstart Jar, `aem-<tier>_<environment>-p<port number>.jar` indique comment il démarrera. Une fois qu’AEM a commencé dans un niveau, un auteur ou une publication spécifique, il ne peut pas être modifié dans un autre niveau. Pour ce faire, le dossier `crx-Quickstart` généré lors de la première exécution doit être supprimé et Quickstart Jar doit être exécuté à nouveau. L’environnement et les ports peuvent être modifiés, mais ils nécessitent l’arrêt/le démarrage de l’instance d’AEM locale.

La modification des environnements, `dev`, `stage` et `prod`, peut s’avérer utile pour les développeurs afin de s’assurer que les configurations spécifiques à un environnement sont correctement définies et résolues par AEM. Il est recommandé que le développement local soit principalement effectué par rapport au mode d’exécution de l’environnement par défaut `dev`.

Les permutations disponibles sont les suivantes :

+ `aem-author-p4502.jar`
   + En tant qu’auteur en mode d’exécution de développement sur le port 4502
+ `aem-author_dev-p4502.jar`
   + En tant qu’auteur en mode d’exécution de développement sur le port 4502 (identique à `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + En tant qu’auteur en mode d’exécution intermédiaire sur le port 4502
+ `aem-author_prod-p4502.jar`
   + En tant qu’auteur en mode d’exécution Production sur le port 4502
+ `aem-publish-p4503.jar`
   + En tant qu’auteur en mode d’exécution de développement sur le port 4503
+ `aem-publish_dev-p4503.jar`
   + En tant qu’auteur en mode d’exécution de développement sur le port 4503 (identique à `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + En tant qu’auteur en mode d’exécution intermédiaire sur le port 4503
+ `aem-publish_prod-p4503.jar`
   + En tant qu’auteur en mode d’exécution Production sur le port 4503

Notez que le numéro de port peut être n’importe quel port disponible sur la machine de développement locale, par convention :

+ Le port __4502__ est utilisé pour le __service AEM Author local__
+ Le port __4503__ est utilisé pour le __service AEM Publish local__

Pour les modifier, il peut être nécessaire d’apporter des modifications aux configurations AEM SDK.

## Arrêt d’une exécution d’AEM locale

Pour arrêter une exécution d’AEM locale, qu’il s’agisse d’un service AEM Author ou de publication, ouvrez la fenêtre de ligne de commande qui a été utilisée pour démarrer l’exécution AEM, puis appuyez sur `Ctrl-C`. Attendez la fermeture de AEM. Une fois le processus d’arrêt terminé, l’invite de ligne de commande est disponible.

## Tâches facultatives de configuration de l’exécution d’AEM locale

+ __Les variables d’environnement de configuration OSGi et les__ variables secrètes sont  [spécialement définies pour l’exécution locale AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=fr#local-development), plutôt que de les gérer à l’aide de l’interface de ligne de commande aio.

## Quand mettre à jour le fichier Quickstart Jar

Mettez à jour le SDK AEM au moins une fois par mois ou peu après, le dernier jeudi de chaque mois, qui est la cadence de publication d’AEM en tant que &quot;versions de fonctionnalités&quot; Cloud Service.

>[!WARNING]
>
> La mise à jour du fichier Quickstart Jar vers une nouvelle version nécessite de remplacer l’ensemble de l’environnement de développement local, ce qui entraîne la perte de tout le code, la configuration et le contenu dans les référentiels d’AEM locaux. Assurez-vous que tout code, configuration ou contenu qui ne doit pas être détruit est validé en toute sécurité dans Git ou exporté depuis l’instance d’AEM locale en tant que modules AEM.

### Comment éviter la perte de contenu lors de la mise à niveau du SDK AEM

La mise à niveau du SDK AEM crée effectivement un nouveau runtime AEM, y compris un nouveau référentiel, ce qui signifie que toutes les modifications apportées au référentiel d’un SDK d’AEM précédent sont perdues. Vous trouverez ci-dessous des stratégies viables pour faciliter la conservation du contenu entre les mises à niveau AEM SDK et peuvent être utilisées séparément ou de concert :

1. Créez un module de contenu dédié à contenir des &quot;exemples&quot; de contenu pour faciliter le développement et conservez-le dans Git. Tout contenu qui doit être conservé lors des mises à niveau AEM SDK est conservé dans ce module et redéployé après la mise à niveau du SDK AEM.
1. Utilisez [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) avec la directive `includepaths` pour copier le contenu du référentiel SDK AEM précédent vers le nouveau référentiel SDK AEM.
1. Sauvegardez tout contenu à l’aide d’AEM Package Manager et des packages de contenu sur le kit SDK AEM précédent, puis réinstallez-les sur le nouveau SDK d’AEM.

N’oubliez pas que l’utilisation des approches ci-dessus pour conserver le code entre les mises à niveau AEM SDK indique un anti-modèle de développement. Le code non disponible doit provenir de votre IDE de développement et être transmis dans AEM SDK via des déploiements.

## Résolution des problèmes

## Si vous double-cliquez sur le fichier Jar de démarrage rapide, une erreur s’affiche.{#troubleshooting-double-click}

Lorsque vous double-cliquez sur le fichier Quickstart Jar pour démarrer, un modal d’erreur s’affiche pour empêcher AEM de démarrer localement.

![Dépannage - Double-cliquez sur le fichier Quickstart Jar](./assets/aem-runtime/troubleshooting__double-click.png)

Cela est dû au fait qu’AEM en tant que Cloud Service Quickstart Jar ne prend pas en charge le double-clic de Quickstart Jar pour commencer AEM localement. Vous devez plutôt exécuter le fichier Jar à partir de cette ligne de commande.

Pour démarrer le service AEM Author, accédez au répertoire contenant le fichier Quickstart Jar et exécutez la commande :`cd`

`$ java -jar aem-author-p4502.jar`

ou, pour démarrer le service AEM Publish, `cd` dans le répertoire contenant le fichier Quickstart Jar et exécutez la commande :

`$ java -jar aem-publish-p4503.jar`

## Le démarrage du fichier Quickstart Jar à partir de la ligne de commande annule immédiatement.{#troubleshooting-java-8}

Lors du démarrage du fichier Quickstart Jar à partir de la ligne de commande, le processus s’arrête immédiatement et le service AEM ne démarre pas, avec l’erreur suivante :

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

En effet, AEM as a Cloud Service nécessite Java SDK 11 et vous exécutez une version différente, probablement Java 8. Pour résoudre ce problème, téléchargez et installez [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Une fois le SDK Java 11 installé, vérifiez qu’il s’agit de la version principale en exécutant les éléments suivants à partir de la ligne de commande.

Une fois le SDK Java 11 installé, vérifiez qu’il s’agit de la version principale en exécutant la commande à partir de la ligne de commande :

+ Windows : `java -version`
+ macOS / Linux : `java --version`

## Ressources supplémentaires

+ [Télécharger AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Télécharger Docker](https://www.docker.com/)
+ [Documentation du Dispatcher Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr)
