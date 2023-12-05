---
title: Configurer l’exécution locale du SDK AEM pour le développement AEM as a Cloud Service
description: Configurez l’exécution locale du SDK AEM à l’aide du fichier Quickstart Jar du SDK d’AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 563
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1695'
ht-degree: 100%

---

# Configurer l’exécution locale du SDK AEM {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Exécution locale d’AEM"
>abstract="Il est possible d&#39;exécuter localement Adobe Experience Manager (AEM) à l&#39;aide du SDK d&#39;AEM as a Cloud Service Quickstart Jar. Cela permet aux développeurs de déployer et de tester du code personnalisé, des configurations et du contenu avant de le soumettre à un contrôle de sources et de le déployer dans un environnement AEM as a Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=fr" text="SDK AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?lang=fr" text="Télécharger le SDK AEM as a Cloud Service"

Il est possible d&#39;exécuter localement Adobe Experience Manager (AEM) à l&#39;aide du SDK d&#39;AEM as a Cloud Service Quickstart Jar. Cela permet aux développeurs de déployer et de tester du code personnalisé, des configurations et du contenu avant de le soumettre à un contrôle de sources et de le déployer dans un environnement AEM as a Cloud Service.

Notez que `~` est utilisé comme raccourci pour le répertoire de l’utilisateur ou de l’utilisatrice. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

## Installer Java

Experience Manager est une application Java qui requiert donc le SDK Oracle Java pour prendre en charge l’outil de développement.

1. [Télécharger et installer le dernier SDK Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
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

![Java.](./assets/aem-runtime/java.png)

## Télécharger le SDK AEM as a Cloud Service

Le SDK AEM as a Cloud Service, ou SDK AEM, contient le fichier Quickstart Jar utilisé pour exécuter localement l’instance de création et de publication AEM pour le développement, ainsi que la version compatible des outils Dispatcher.

1. Connectez-vous à [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) avec votre Adobe ID.
   + Votre organisation Adobe __doit__ être configurée pour AEM as a Cloud Service afin de télécharger le SDK AEM as a Cloud Service.
1. Accédez à l’onglet __AEM as a Cloud Service__.
1. Effectuez un tri par __date de publication__ dans l’ordre __décroissant__
1. Cliquez sur la dernière ligne de résultats __SDK AEM__.
1. Consultez et acceptez le CLUF, puis appuyez sur le bouton __Télécharger__

## Extrayez le fichier Quickstart Jar du fichier zip du SDK AEM.

1. Décompressez le fichier `aem-sdk-XXX.zip` téléchargé.

## Configurer le service de création AEM local{#set-up-local-aem-author-service}

Le service de création AEM local fournit aux développeurs et développeuses une expérience locale que les spécialistes du marketing numérique/auteurs et autrices de contenu peuvent partager pour créer et gérer du contenu.  Le service de création AEM est conçu à la fois comme un environnement de création et de prévisualisation. Il permet d’effectuer la plupart des validations du développement de fonctionnalités, ce qui en fait un élément essentiel du processus de développement local.

1. Créez le dossier `~/aem-sdk/author`.
1. Copiez le fichier __Quickstart JAR__ vers `~/aem-sdk/author` et renommez-le `aem-author-p4502.jar`.
1. Démarrez le service de création AEM local en exécutant les éléments suivants à partir de la ligne de commande :
   + `java -jar aem-author-p4502.jar`
      + Indiquez le mot de passe d’administration `admin`. Tout mot de passe d’administration est acceptable, mais il est recommandé d’utiliser celui par défaut pour le développement local afin de réduire la nécessité de reconfiguration.

   Il est *impossible* de démarrer le fichier Quickstart JAR AEM as Cloud Service en [double-cliquant](#troubleshooting-double-click) dessus.
1. Accédez au service de création AEM local en saisissant l’adresse [http://localhost:4502](http://localhost:4502) dans un navigateur Web.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## Configurer le service de publication AEM local

Le service de publication AEM local fournit aux développeurs et développeuses l’expérience locale qu’auront les utilisateurs et utilisatrices finaux d’AEM, comme la navigation sur le site Web hébergé sur AEM. Le service de publication AEM local est important, dans la mesure où il s’intègre aux [Outils du Dispatcher](./dispatcher-tools.md) du SDK AEM et permet aux développeurs et développeuses de tester et d’affiner l’expérience de contact des utilisateurs et utilisatrices finaux.

1. Créez le dossier `~/aem-sdk/publish`.
1. Copiez le fichier __Quickstart JAR__ vers `~/aem-sdk/publish` et renommez-le `aem-publish-p4503.jar`.
1. Démarrez le service de publication AEM local en exécutant les éléments suivants à partir de la ligne de commande :
   + `java -jar aem-publish-p4503.jar`
      + Indiquez le mot de passe d’administration `admin`. Tout mot de passe d’administration est acceptable, mais il est recommandé d’utiliser celui par défaut pour le développement local afin de réduire la nécessité de reconfiguration.

   Il est *impossible* de démarrer le fichier Quickstart JAR AEM as Cloud Service en [double-cliquant](#troubleshooting-double-click) dessus.
1. Accédez au service de publication AEM local à l’adresse [http://localhost:4503](http://localhost:4503) dans un navigateur Web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## Configurer des services AEM locaux en mode Version préliminaire

L’exécution locale d’AEM peut être démarrée en [mode Version préliminaire](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=fr) pour que les personnes affectées au développement puissent utiliser les fonctionnalités de la prochaine version d’AEM as a Cloud Service. La version préliminaire est activée en transmettant l’argument `-r prerelease` sur le premier démarrage de l’exécution locale d’AEM. Vous pouvez l’utiliser avec les services de création et de publications locaux d’AEM.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## Simuler la distribution de contenu {#content-distribution}

Dans un environnement Cloud Service réel, le contenu est distribué du service de création au service de publication à l’aide de la [Distribution de contenu Sling](https://sling.apache.org/documentation/bundles/content-distribution.html) et du pipeline Adobe. Le [pipeline Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=fr#content-distribution) est un microservice isolé disponible uniquement dans l’environnement cloud.

Pendant le développement, il peut être souhaitable de simuler la distribution du contenu à l’aide du service de création et de publication local. Pour ce faire, activez les agents de réplication hérités.

>[!NOTE]
>
> Les agents de réplication ne peuvent être utilisés que dans le fichier Quickstart JAR local et ne fournissent qu’une simulation de la distribution de contenu.

1. Connectez-vous au service de **création** et accédez à [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Cliquez sur **Agent par défaut (publication)** pour ouvrir l’agent de réplication par défaut.
1. Cliquez sur **Modifier** pour ouvrir la configuration de l’agent.
1. Dans l’onglet **Paramètres**, mettez à jour les champs suivants :

   + **Activé** - Cochez true
   + **ID utilisateur de l’agent** - Laissez ce champ vide

   ![Configuration de l’agent de réplication - Paramètres](assets/aem-runtime/settings-config.png)

1. Dans l’onglet **Transport**, mettez à jour les champs suivants :

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Utilisateur ou utilisatrice** - `admin`
   + **Mot de passe** - `admin`

   ![Configuration de l’agent de réplication - Transport](assets/aem-runtime/transport-config.png)

1. Cliquez sur **OK** pour enregistrer la configuration et activer l’agent de réplication **par défaut**.
1. Vous pouvez maintenant apporter des modifications au contenu sur le service de création et les publier sur le service de publication.

![Publication de la page.](assets/aem-runtime/publish-page-changes.png)

## Modes de démarrage du fichier Quickstart Jar

Le nom du fichier Quickstart Jar, `aem-<tier>_<environment>-p<port number>.jar`, indique le mode de démarrage. Si AEM est lancé dans un niveau spécifique, création ou publication, il est impossible de passer à un autre niveau. Pour ce faire, le dossier `crx-Quickstart` généré lors de la première exécution doit être supprimé et le fichier Quickstart Jar doit être exécuté à nouveau. L’environnement et les ports peuvent être modifiés, mais ils nécessitent l’arrêt/le démarrage de l’instance AEM locale.

Les changements d’environnement, `dev`, `stage` et `prod`, peuvent s’avérer utile pour les développeurs et développeuses afin de s’assurer que les configurations spécifiques à un environnement sont correctement définies et résolues par AEM. Il est recommandé d’effectuer principalement le développement local par rapport au mode d’exécution de l’environnement `dev` par défaut.

Les permutations disponibles sont les suivantes :

| Nom de fichier Quickstart Jar | Description du mode |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | En tant que service de création en mode d’exécution de développement sur le port 4502 |
| `aem-author_dev-p4502.jar` | En tant que service de création en mode d’exécution de développement sur le port 4502 (identique à `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | En tant que service de création en mode d’exécution d’évaluation sur le port 4502 |
| `aem-author_prod-p4502.jar` | En tant que service de création en mode d’exécution de production sur le port 4502 |
| `aem-publish-p4503.jar` | En tant que service de publication en mode d’exécution de développement sur le port 4503 |
| `aem-publish_dev-p4503.jar` | En tant que service de publication en mode d’exécution de développement sur le port 4503 (identique à `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | En tant que service de publication en mode d’exécution d’évaluation sur le port 4503 |
| `aem-publish_prod-p4503.jar` | En tant que service de publication en mode d’exécution de production sur le port 4503 |

Notez que le numéro de port peut être n’importe quel port disponible sur la machine de développement locale, mais par convention :

+ Le port __4502__ est utilisé pour le __service de création AEM local__.
+ Le port __4503__ est utilisé pour le __service de publication AEM local__.

Pour les modifier, il peut être nécessaire d’apporter des modifications aux configurations SDK AEM.

## Arrêt d’une exécution locale d’AEM

Pour arrêter une exécution locale d’AEM, qu’il s’agisse d’un service de création ou de publication AEM, ouvrez la fenêtre de ligne de commande qui a été utilisée pour démarrer l’exécution d’AEM, puis appuyez sur `Ctrl-C`. Attendez la fermeture d’AEM. Une fois le processus d’arrêt terminé, l’invite de ligne de commande est disponible.

## Tâches facultatives de configuration de l’exécution locale d’AEM

+ Les __variables secrètes et les variables d’environnement de configuration OSGi__ sont [spécialement définies pour l’exécution locale d’AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=fr#local-development), au lieu d’être gérées à l’aide de l’interface de ligne de commande AIO.

## Quand mettre à jour le fichier Quickstart Jar

Mettez à jour le SDK AEM au moins une fois par mois le dernier jeudi de chaque mois, ou quelque temps après. Il s’agit de la cadence de mise à jour « des fonctionnalités » d’AEM as a Cloud Service.

>[!WARNING]
>
> La mise à niveau du fichier Quickstart Jar vers une nouvelle version nécessite le remplacement de l’ensemble de l’environnement de développement local, ce qui entraîne la perte de l’intégralité du code, de la configuration et du contenu dans les référentiels AEM locaux. Assurez-vous que l’intégralité du code, de la configuration ou du contenu ne devant pas être détruit est validé en toute sécurité dans Git ou exporté depuis les instances AEM locales en tant que packages AEM.

### Comment éviter la perte de contenu lors de la mise à niveau du SDK AEM ?

La mise à niveau du SDK AEM crée effectivement une toute nouvelle exécution d’AEM, y compris un nouveau référentiel, ce qui signifie que toutes les modifications apportées au référentiel d’un SDK AEM précédent sont perdues. Vous trouverez ci-dessous des stratégies viables pour faciliter la conservation du contenu entre les mises à niveau du SDK AEM et qui peuvent être utilisées séparément ou ensemble :

1. Créez un package de contenu dédié à contenir des « exemples » de contenu pour faciliter le développement, et conservez-le dans Git. Tout contenu qui doit être conservé lors des mises à niveau du SDK AEM est conservé dans ce package et redéployé après la mise à niveau du SDK AEM.
1. Utilisez [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) avec la directive `includepaths` pour copier le contenu du référentiel SDK AEM précédent vers le nouveau référentiel SDK AEM.
1. Sauvegardez tout contenu à l’aide du gestionnaire de modules d’AEM et des packages de contenu sur le SDK AEM précédent, puis réinstallez-les sur le nouveau SDK AEM.

N’oubliez pas que l’utilisation des approches ci-dessus pour conserver le code entre les mises à niveau SDK AEM indique un anti-modèle de développement. Le code non jetable doit provenir de votre IDE de développement et être transféré dans le SDK AEM via des déploiements.

## Résolution des problèmes

### Un double-clic sur le fichier Quickstart Jar entraîne une erreur.{#troubleshooting-double-click}

Lorsque vous double-cliquez sur le fichier Quickstart Jar pour démarrer, une fenêtre modale d’erreur s’affiche et empêche AEM de démarrer localement.

![Résolution de problèmes - Double-cliquer sur le fichier Quickstart Jar](./assets/aem-runtime/troubleshooting__double-click.png)

Cela est dû au fait que le fichier Quickstart Jar d’AEM as a Cloud Service ne prend pas en charge le double-clic sur le fichier Quickstart Jar pour démarrer AEM localement. Vous devez plutôt exécuter le fichier Jar à partir de cette ligne de commande.

Pour démarrer le service de création AEM, utilisez `cd` dans le répertoire contenant le fichier Jar de démarrage rapide et exécutez la commande :

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

ou, pour démarrer le service de publication AEM, utilisez `cd` dans le répertoire contenant le fichier Jar de démarrage rapide et exécutez la commande :

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### Arrêt immédiat lors du démarrage du fichier Jar de démarrage rapide à partir de la ligne de commande{#troubleshooting-java-8}

Lors du démarrage du fichier Jar de démarrage rapide à partir de la ligne de commande, le processus s’arrête immédiatement et le service AEM ne démarre pas, avec l’erreur suivante :

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

En effet, AEM as a Cloud Service nécessite le SDK Java 11 et vous exécutez une version différente, probablement Java 8. Pour résoudre ce problème, téléchargez et installez [Oracle SDK Java 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).

Une fois le SDK Oracle Java 11 installé, vérifiez qu’il s’agit de la version active en exécutant la commande à partir de la ligne de commande :

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

## Ressources supplémentaires

+ [Télécharger le SDK d’AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Télécharger Docker](https://www.docker.com/)
+ [Documentation du Dispatcher Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr)
