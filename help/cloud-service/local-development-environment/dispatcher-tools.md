---
title: Configurer des outils du Dispatcher pour le développement AEM as a Cloud Service
description: Les outils du Dispatcher du SDK AEM facilitent le développement local de projets Adobe Experience Manager (AEM) en facilitant l’installation, l’exécution et le dépannage local du Dispatcher.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: 2a412126ac7a67a756d4101d56c1715f0da86453
workflow-type: tm+mt
source-wordcount: '1695'
ht-degree: 100%

---

# Configurer les outils du Dispatcher local {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Outils du Dispatcher local"
>abstract="Le Dispatcher fait partie intégrante de l’architecture globale d’Experience Manager et doit faire partie de la configuration de développement local. Le SDK AEM as a Cloud Service comprend la version recommandée des outils du Dispatcher qui facilite la configuration, la validation et la simulation locale du Dispatcher."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=fr" text="Dispatcher en mode cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?lang=fr" text="Télécharger le SDK AEM as a Cloud Service"

Le Dispatcher Adobe Experience Manager (AEM) est un module de serveur web HTTP Apache qui fournit une couche de sécurité et de performances entre le réseau CDN et le niveau de publication d’AEM. Il fait partie intégrante de l’architecture globale d’Experience Manager et doit faire partie de la configuration de développement local.

Le SDK AEM as a Cloud Service comprend la version recommandée des outils du Dispatcher qui facilite la configuration, la validation et la simulation locale du Dispatcher. Les outils du Dispatcher se composent des éléments suivants :

+ un ensemble de base de fichiers de configuration du serveur web Apache HTTP et du Dispatcher, situé dans le répertoire `.../dispatcher-sdk-x.x.x/src` ;
+ un outil d’interface de ligne de commande du validateur de configuration, situé dans le répertoire `.../dispatcher-sdk-x.x.x/bin/validate` ;
+ un outil d’interface de ligne de commande de génération de configuration situé dans le répertoire `.../dispatcher-sdk-x.x.x/bin/validator` ;
+ un outil d’interface de ligne de commande pour le déploiement des configurations situé dans le répertoire `.../dispatcher-sdk-x.x.x/bin/docker_run` ;
+ des fichiers de configuration non modifiables remplaçant l’interface de ligne de commande, situés dans le répertoire `.../dispatcher-sdk-x.x.x/bin/update_maven` ;
+ une image Docker qui exécute le serveur web Apache HTTP avec le module Dispatcher.

Notez que `~` est utilisé comme raccourci pour le répertoire de l’utilisateur ou de l’utilisatrice. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

>[!NOTE]
>
> Les vidéos de cette page ont été enregistrées sur macOS. Les utilisateurs et utilisatrices de Windows peuvent également suivre leurs indications, mais en utilisant les commandes Windows équivalentes des outils Dispatcher, fournies avec chaque vidéo.

## Conditions préalables

1. Les utilisateurs et utilisatrices de Windows doivent disposer de Windows 10 Professionel (ou une version prenant en charge Docker).
1. Installez le [fichier Jar de démarrage rapide de publication d’Experience Manager](./aem-runtime.md) sur la machine de développement locale.

+ Vous pouvez éventuellement installer la dernière version du [site de référence AEM](https://github.com/adobe/aem-guides-wknd/releases) sur le service de publication AEM local. Ce site est utilisé dans ce tutoriel pour visualiser un Dispatcher en cours de fonctionnement.

1. Installez et démarrez la dernière version de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5 ou ultérieure/Docker Engine v19.03.9 ou ultérieure) sur l’ordinateur de développement local.

## Télécharger les outils du Dispatcher (dans le cadre du SDK AEM)

Le SDK AEM as a Cloud Service, ou SDK AEM, contient les outils du Dispatcher utilisés pour exécuter le serveur web Apache HTTP avec le module Dispatcher localement pour le développement, ainsi que le fichier Jar de démarrage rapide compatible.

Si le SDK AEM as a Cloud Service a déjà été téléchargé vers la [configuration de l’exécution d’AEM locale](./aem-runtime.md), il n’est pas nécessaire de le télécharger de nouveau.

1. Connectez-vous à [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) avec votre Adobe ID.
   + Votre organisation Adobe __doit__ être configurée pour AEM as a Cloud Service afin de télécharger le SDK AEM as a Cloud Service.
1. Cliquez sur la dernière ligne de résultats __AEM SDK__ à télécharger.

## Extrayez les outils du Dispatcher du fichier zip AEM SDK.

>[!TIP]
>
> Les utilisateurs et utilisatrices de Windows ne peuvent pas mettre d’espaces ni de caractères spéciaux dans le chemin d’accès au dossier contenant les outils du Dispatcher local. S’il y a des espaces dans le chemin d’accès, `docker_run.cmd` échouera.

La version des outils du Dispatcher est différente de celle d’AEM SDK. Assurez-vous que la version des outils du Dispatcher est fournie via la version du SDK AEM correspondant à la version d’AEM as a Cloud Service.

1. Décompressez le fichier `aem-sdk-xxx.zip` téléchargé.
1. Décompressez les outils du Dispatcher dans `~/aem-sdk/dispatcher`.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Décompressez `aem-sdk-dispatcher-tools-x.x.x-windows.zip` dans `C:\Users\<My User>\aem-sdk\dispatcher` (en créant les dossiers manquants si nécessaire).

>[!TAB Linux]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Toutes les commandes émises ci-dessous supposent que le répertoire de travail actuel contient le contenu des outils du Dispatcher en développement.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires.*

## Comprendre les fichiers de configuration de Dispatcher

>[!TIP]
> Les projets Experience Manager créés à partir de l’[Archétype Maven de projet AEM](https://github.com/adobe/aem-project-archetype) sont prérenseignés par cet ensemble de fichiers de configuration de Dispatcher. Il n’est donc pas nécessaire de les copier depuis le dossier src des outils du Dispatcher.

Les outils du Dispatcher fournissent un ensemble de fichiers de configuration du serveur web Apache HTTP et du Dispatcher qui définissent le comportement de tous les environnements, y compris du développement local.

Ces fichiers sont destinés à être copiés dans un projet Maven Experience Manager vers le dossier `dispatcher/src` s’ils n’existent pas déjà dans le projet Maven Experience Manager.

Une description complète des fichiers de configuration est disponible dans les outils du Dispatcher décompressés sous la forme `dispatcher-sdk-x.x.x/docs/Config.html`.

## Valider les configurations

Facultativement, les configurations du serveur web du Dispatcher et Apache (via `httpd -t`) peuvent être validées à l’aide du script `validate` (à ne pas confondre avec l’exécutable `validator`). Le script `validate` constitue un moyen pratique d’exécuter les [trois phases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=fr) du `validator`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Exécuter Dispatcher localement

AEM Dispatcher est exécuté localement à l’aide de Docker par rapport aux fichiers `src` de configuration de Dispatcher et du serveur web Apache.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

L’exécutable `docker_run_hot_reload` est préférable par rapport à `docker_run`, car il recharge les fichiers de configuration au fur et à mesure de leur modification, sans avoir à arrêter et redémarrer manuellement `docker_run`. `docker_run` peut aussi être utilisé, mais cela nécessite un arrêt et un redémarrage manuels de `docker_run` lorsque les fichiers de configuration sont modifiés.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

L’exécutable `docker_run_hot_reload` est préférable par rapport à `docker_run`, car il recharge les fichiers de configuration au fur et à mesure de leur modification, sans avoir à arrêter et redémarrer manuellement `docker_run`. `docker_run` peut aussi être utilisé, mais cela nécessite un arrêt et un redémarrage manuels de `docker_run` lorsque les fichiers de configuration sont modifiés.

>[!ENDTABS]

`<aem-publish-host>` peut être défini sur `host.docker.internal`, un nom DNS spécial que Docker fournit dans le conteneur qui résout l’adresse IP de l’ordinateur hôte. Si `host.docker.internal` ne résout rien, reportez-vous à la section [dépannage](#troubleshooting-host-docker-internal) ci-dessous.

Par exemple, pour démarrer le conteneur Docker de Dispatcher à l’aide des fichiers de configuration par défaut fournis par les outils du Dispatcher :

Démarrez le conteneur Docker de Dispatcher en indiquant le chemin d’accès au dossier src de configuration de Dispatcher :

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

Le service de publication du SDK d’AEM as a Cloud Service, s’exécutant localement sur le port 4503, est disponible via Dispatcher sur `http://localhost:8080`.

Pour exécuter les outils du Dispatcher par rapport à la configuration Dispatcher d’un projet d’Experience Manager, pointez sur le dossier `dispatcher/src` de votre projet.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Journaux des outils du Dispatcher

Les journaux de Dispatcher sont utiles lors du développement local pour comprendre si et pourquoi les requêtes HTTP sont bloquées. Le niveau de journalisation peut être défini en ajoutant un préfixe à l’exécution de `docker_run` avec les paramètres d’environnement.

Les journaux des outils du Dispatcher sont émis vers Standard Out lorsque `docker_run` est exécuté.

Les paramètres utiles pour le débogage de Dispatcher incluent :

+ `DISP_LOG_LEVEL=Debug` définit la journalisation du module Dispatcher sur le niveau de débogage.
   + La valeur par défaut est : `Warn`
+ `REWRITE_LOG_LEVEL=Debug` définit la journalisation du module de réécriture du serveur web Apache HTTP au niveau de débogage.
   + La valeur par défaut est : `Warn`
+ `DISP_RUN_MODE` définit le « mode d’exécution » de l’environnement de Dispatcher, en chargeant les fichiers de configuration de Dispatcher des modes d’exécution correspondants.
   + La valeur par défaut est `dev`.
+ Valeurs valides : `dev`, `stage`, ou `prod`

Un ou plusieurs paramètres peuvent être transmis à `docker_run`.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Accès aux fichiers journaux

Le serveur web Apache et les journaux de Dispatcher AEM sont directement accessibles dans le conteneur Docker :

+ [Accéder aux journaux dans le conteneur Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copier les journaux Docker vers le système de fichiers local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quand mettre à jour les outils du Dispatcher{#dispatcher-tools-version}

Les versions des outils du Dispatcher s’incrémentent moins fréquemment qu’Experience Manager. Par conséquent, les outils du Dispatcher nécessitent moins de mises à jour dans l’environnement de développement local.

La version recommandée des outils du Dispatcher est celle qui est fournie avec le SDK AEM as a Cloud Service qui correspond à la version d’Experience Manager as a Cloud Service. La version d’AEM as a Cloud Service est disponible via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Environnements__, selon l’environnement spécifié par le libellé __Version d’AEM__.

![Version d’Experience Manager.](./assets/dispatcher-tools/aem-version.png)

*Notez que la version des outils du Dispatcher ne correspond pas à la version d’Experience Manager.*

## Comment mettre à jour les configurations de base Apache et de Dispatcher

La configuration de base Apache et de Dispatcher est améliorée régulièrement et publiée avec la version du SDK AEM as a Cloud Service. Il est recommandé d’incorporer des améliorations de configuration de base dans votre projet AEM et d’éviter la [validation locale](#validate-configurations) et les échecs du pipeline Cloud Manager. Mettez-les à jour à l’aide du script `update_maven.sh` du dossier `.../dispatcher-sdk-x.x.x/bin`.

>[!VIDEO](https://video.tv.adobe.com/v/3416744?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires.*


Supposons que vous ayez créé un projet AEM dans le passé en utilisant l’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype), et que les configurations de base Apache et de Dispatcher étaient à jour. En utilisant ces configurations de base, vos configurations spécifiques au projet ont été créées en réutilisant et en copiant des fichiers comme `*.vhost`, `*.conf`, `*.farm` et `*.any` des dossiers `dispatcher/src/conf.d` et `dispatcher/src/conf.dispatcher.d`. La validation de votre Dispatcher local et les pipelines Cloud Manager fonctionnaient correctement.

Pendant ce temps, les configurations de base Apache et de Dispatcher ont été améliorées pour diverses raisons, telles que de nouvelles fonctionnalités, des correctifs de sécurité et l’optimisation. Elles sont publiées par le biais d’une version plus récente des outils du Dispatcher dans le cadre de la version d’AEM as a Cloud Service.

Lors de la validation de vos configurations de Dispatcher spécifiques au projet par rapport à la dernière version des outils du Dispatcher, elles commencent à échouer. Pour résoudre ce problème, les configurations de base doivent être mises à jour en suivant les étapes ci-dessous :

+ Vérifiez que la validation échoue par rapport à la dernière version des outils du Dispatcher.

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Mettez à jour les fichiers non modifiables à l’aide du script `update_maven.sh`.

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ Vérifiez les fichiers non modifiables mis à jour comme `dispatcher_vhost.conf`, `default.vhost`, et `default.farm` et, si nécessaire, apportez les modifications pertinentes à vos fichiers personnalisés qui sont dérivés de ces fichiers.

+ Revalidez les configurations, et cela devrait fonctionner.

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ Après la vérification locale des modifications, validez les fichiers de configuration mis à jour.

## Résolution des problèmes

### docker_run renvoie un message « En attente jusqu’à ce que host.docker.internal soit disponible ».{#troubleshooting-host-docker-internal}

`host.docker.internal` est un nom d’hôte fourni au conteneur Docker qui résout l’hôte. Pour docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)) :

> À compter de la version 18.03 de Docker, il est recommandé de se connecter au nom DNS spécial host.docker.internal, qui correspond à l’adresse IP interne utilisée par l’hôte.

Lorsque `bin/docker_run src host.docker.internal:4503 8080` renvoie le message __En attente jusqu’à ce que host.docker.internal soit disponible__ :

1. Assurez-vous que la version installée de Docker est 18.03 ou supérieure.
1. Votre ordinateur local est peut-être configuré de manière à empêcher l’enregistrement/la résolution du nom `host.docker.internal`. Utilisez plutôt votre adresse IP locale.

>[!BEGINTABS]

>[!TAB macOS]

+ Depuis le terminal, exécutez `ifconfig` et enregistrer l’adresse IP __inet__ de l’hôte, généralement l’appareil __en0__.
+ Exécutez `docker_run` en utilisant l’adresse IP de l’hôte : `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ Dans l’invite de commande, exécutez `ipconfig`, et enregistrez l’__adresse IPv4__ de l’hôte de l’ordinateur hôte.
+ Ensuite, exécutez `docker_run` en utilisant cette adresse IP : `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux]

+ Depuis le terminal, exécutez `ifconfig` et enregistrer l’adresse IP __inet__ de l’hôte, généralement l’appareil __en0__.
+ Exécutez `docker_run` en utilisant l’adresse IP de l’hôte : `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### Exemple d’erreur

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Ressources supplémentaires

+ [Télécharger le SDK d’AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Télécharger Docker](https://www.docker.com/)
+ [Télécharger le site web de référence d’AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentation du Dispatcher Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr)
