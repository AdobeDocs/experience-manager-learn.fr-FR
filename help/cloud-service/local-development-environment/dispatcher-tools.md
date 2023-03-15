---
title: Configuration des outils de Dispatcher pour AEM développement as a Cloud Service
description: Les outils Dispatcher du SDK AEM facilitent le développement local de projets Adobe Experience Manager (AEM) en facilitant l’installation, l’exécution et le dépannage local de Dispatcher.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: eb31c5fb79e01e1c363fc153355e8d92d1a54021
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 3%

---

# Configuration des outils Dispatcher locaux {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Outils du Dispatcher local"
>abstract="Dispatcher fait partie intégrante de l’architecture globale du Experience Manager et doit faire partie de la configuration du développement local. Le SDK as a Cloud Service d’AEM comprend la version recommandée des outils Dispatcher, qui facilite la configuration, la validation et la simulation locale de Dispatcher."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=fr" text="Dispatcher en mode cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Télécharger AEM SDK as a Cloud Service"

Dispatcher d’Adobe Experience Manager (AEM) est un module de serveur web Apache HTTP qui fournit une couche de sécurité et de performances entre le réseau de diffusion de contenu et le niveau Publication AEM. Dispatcher fait partie intégrante de l’architecture globale du Experience Manager et doit faire partie de la configuration du développement local.

Le SDK as a Cloud Service d’AEM comprend la version recommandée des outils Dispatcher, qui facilite la configuration, la validation et la simulation locale de Dispatcher. Les outils de Dispatcher se composent des éléments suivants :

+ un ensemble de base de fichiers de configuration du serveur web Apache HTTP et du Dispatcher, situé à l’adresse `.../dispatcher-sdk-x.x.x/src`
+ un outil d’interface de ligne de commande du validateur de configuration situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/validate`
+ un outil d’interface de ligne de commande de génération de configuration situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/validator`
+ un outil d’interface de ligne de commande pour le déploiement des configurations situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ des fichiers de configuration non modifiables remplaçant l’outil d’interface de ligne de commande, situé à l’emplacement `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ une image Docker qui exécute le serveur web Apache HTTP avec le module Dispatcher ;

Notez que `~` est utilisé comme abrégé pour le répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

>[!NOTE]
>
> Les vidéos de cette page ont été enregistrées sur macOS. Les utilisateurs de Windows peuvent suivre, mais utiliser les commandes Windows équivalentes des outils Dispatcher, fournies avec chaque vidéo.

## Prérequis

1. Les utilisateurs de Windows doivent utiliser Windows 10 Professional (ou une version prenant en charge Docker)
1. Installer [Fichier Jar de démarrage rapide de publication Experience Manager](./aem-runtime.md) sur la machine de développement locale.

+ Si vous le souhaitez, vous pouvez installer la dernière [AEM site web de référence](https://github.com/adobe/aem-guides-wknd/releases) sur le service AEM Publish local. Ce site web est utilisé dans ce tutoriel pour visualiser un Dispatcher fonctionnel.

1. Installez et démarrez la dernière version de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) sur l’ordinateur de développement local.

## Téléchargement des outils de Dispatcher (dans le cadre du SDK AEM)

Le SDK as a Cloud Service AEM, ou SDK AEM, contient les outils Dispatcher utilisés pour exécuter le serveur web Apache HTTP avec le module Dispatcher localement pour le développement, et le fichier Jar QuickStart compatible.

Si le SDK as a Cloud Service AEM a déjà été téléchargé vers [configuration de l’exécution d’AEM locale](./aem-runtime.md), il n’est pas nécessaire de le retélécharger.

1. Connectez-vous à [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=1) avec votre Adobe ID
   + Votre organisation Adobe __must__ être configuré pour AEM as a Cloud Service pour télécharger AEM SDK as a Cloud Service
1. Cliquez sur la dernière __AEM SDK__ ligne de résultat à télécharger

## Extrayez les outils Dispatcher du fichier zip du SDK AEM

>[!TIP]
>
> Les utilisateurs de Windows ne peuvent pas avoir d’espaces ni de caractères spéciaux dans le chemin d’accès au dossier contenant les outils du Dispatcher local. S’il existe des espaces dans le chemin, la variable `docker_run.cmd` échoue.

La version des outils de Dispatcher est différente de celle du SDK AEM. Assurez-vous que la version des outils de Dispatcher est fournie via la version AEM SDK correspondant à la version AEM as a Cloud Service.

1. Décompressez le fichier téléchargé. `aem-sdk-xxx.zip` fichier
1. Décompressez les outils Dispatcher dans `~/aem-sdk/dispatcher`

+ Windows : Décompresser `aem-sdk-dispatcher-tools-x.x.x-windows.zip` into `C:\Users\<My User>\aem-sdk\dispatcher` (création de dossiers manquants, si nécessaire)
+ macOS Linux® : Exécution du script shell associé `aem-sdk-dispatcher-tools-x.x.x-unix.sh` pour décompresser les outils de Dispatcher
   + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Toutes les commandes émises ci-dessous supposent que le répertoire de travail actuel contient le contenu des outils Dispatcher en développement.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires.*

## Présentation des fichiers de configuration de Dispatcher

>[!TIP]
> Projets Experience Manager créés à partir de [Archétype Maven de projet AEM](https://github.com/adobe/aem-project-archetype) sont prérenseignés à cet ensemble de fichiers de configuration de Dispatcher. Il n’est donc pas nécessaire de les copier depuis le dossier src des outils Dispatcher.

Les outils de Dispatcher fournissent un ensemble de fichiers de configuration du serveur web Apache HTTP et de Dispatcher qui définissent le comportement de tous les environnements, y compris le développement local.

Ces fichiers sont destinés à être copiés dans un projet Maven Experience Manager vers le `dispatcher/src` s’ils n’existent pas déjà dans le projet Maven Experience Manager.

Une description complète des fichiers de configuration est disponible dans les outils Dispatcher décompressés sous la forme `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validation des configurations

Facultativement, les configurations du serveur web Dispatcher et Apache (via `httpd -t`) peut être validé à l’aide de la variable `validate` (à ne pas confondre avec le `validator` exécutable). Le `validate` Le script offre un moyen pratique d’exécuter la variable [trois phases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=en) de `validator`.

+ Utilisation:
   + Windows : `bin\validate src`
   + macOS Linux® : `./bin/validate.sh ./src`

## Exécution locale de Dispatcher

AEM Dispatcher est exécuté localement à l’aide de Docker par rapport à `src` Fichiers de configuration de Dispatcher et du serveur web Apache.

+ Utilisation:
   + Windows : `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS Linux® : `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

Le `<aem-publish-host>` peut être défini sur `host.docker.internal`, un nom DNS spécial que Docker fournit dans le conteneur qui résout l’adresse IP de l’ordinateur hôte. Si la variable `host.docker.internal` ne résout pas, reportez-vous à la section [dépannage](#troubleshooting-host-docker-internal) ci-dessous.

Par exemple, pour démarrer le conteneur Docker de Dispatcher à l’aide des fichiers de configuration par défaut fournis par les outils Dispatcher :

Démarrez le conteneur Docker de Dispatcher en indiquant le chemin d’accès au dossier src de configuration de Dispatcher :

+ Windows : `bin\docker_run src host.docker.internal:4503 8080`
+ macOS Linux® : `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

Le service de publication du SDK as a Cloud Service, s’exécutant localement sur le port 4503, est disponible via Dispatcher à l’adresse `http://localhost:8080`.

Pour exécuter les outils Dispatcher par rapport à la configuration Dispatcher d’un projet de Experience Manager, pointez sur la variable `dispatcher/src` dossier.

+ Windows :

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS Linux® :

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

## Journaux des outils Dispatcher

Les journaux de Dispatcher sont utiles lors du développement local pour comprendre si et pourquoi les requêtes HTTP sont bloquées. Le niveau de journal peut être défini en ajoutant un préfixe à l’exécution de la variable `docker_run` avec les paramètres d’environnement.

Les journaux des outils de Dispatcher sont émis de manière standard lorsque `docker_run` est exécuté.

Les paramètres utiles pour le débogage de Dispatcher incluent :

+ `DISP_LOG_LEVEL=Debug` définit la journalisation du module Dispatcher sur le niveau de débogage.
   + La valeur par défaut est: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` définit la journalisation du module de réécriture du serveur web Apache HTTP au niveau de débogage.
   + La valeur par défaut est: `Warn`
+ `DISP_RUN_MODE` définit le &quot;mode d’exécution&quot; de l’environnement de Dispatcher, en chargeant les fichiers de configuration de Dispatcher des modes d’exécution correspondants.
   + La valeur par défaut est `dev`.
+ Valeurs valides : `dev`, `stage`ou `prod`

Un ou plusieurs paramètres peuvent être transmis à `docker_run`

+ Windows :

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

+ macOS Linux® :

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

### Accès aux fichiers journaux

Le serveur web Apache et les journaux AEM Dispatcher sont directement accessibles dans le conteneur Docker :

+ [Accès aux journaux dans le conteneur Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copie des journaux Docker vers le système de fichiers local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quand mettre à jour les outils de Dispatcher{#dispatcher-tools-version}

Les versions des outils de Dispatcher s’incrémentent moins fréquemment que le Experience Manager. Par conséquent, les outils de Dispatcher nécessitent moins de mises à jour dans l’environnement de développement local.

La version recommandée des outils de Dispatcher est celle qui est fournie avec le SDK as a Cloud Service AEM qui correspond à la version as a Cloud Service du Experience Manager. La version d’AEM as a Cloud Service est disponible via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Environnements__, par environnement spécifié par le __Version d’AEM__ label

![Version du Experience Manager](./assets/dispatcher-tools/aem-version.png)

*Notez que la version des outils de Dispatcher ne correspond pas à la version du Experience Manager.*

## Comment mettre à jour le jeu de base des configurations Apache et Dispatcher

L’ensemble de base de la configuration Apache et Dispatcher est amélioré régulièrement et publié avec la version as a Cloud Service du SDK AEM. Il est recommandé d’incorporer des améliorations de configuration de base dans votre projet AEM et d’éviter [validation locale](#validate-configurations) et échecs du pipeline de Cloud Manager. Mettez-les à jour à l’aide de la fonction `update_maven.sh` du script `.../dispatcher-sdk-x.x.x/bin` dossier.

>[!VIDEO](https://video.tv.adobe.com/v/3416744/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires.*


Supposons que vous ayez créé un projet AEM dans le passé en utilisant [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype), les configurations Apache de base et Dispatcher étaient actuelles. En utilisant ces configurations de base, vos configurations spécifiques au projet ont été créées en réutilisant et en copiant les fichiers comme `*.vhost`, `*.conf`, `*.farm` et `*.any` de la `dispatcher/src/conf.d` et `dispatcher/src/conf.dispatcher.d` dossiers. La validation de votre Dispatcher local et les pipelines Cloud Manager fonctionnaient correctement.

Pendant ce temps, les configurations Apache de base et Dispatcher ont été améliorées pour diverses raisons, telles que de nouvelles fonctionnalités, des correctifs de sécurité et une optimisation. Elles sont publiées par le biais d’une version plus récente des outils Dispatcher dans le cadre de la version as a Cloud Service d’AEM.

Désormais, lors de la validation de vos configurations Dispatcher spécifiques au projet par rapport à la dernière version des outils Dispatcher, elles commencent à échouer. Pour résoudre ce problème, les configurations de ligne de base doivent être mises à jour en suivant les étapes ci-dessous :

+ Vérifiez que la validation échoue par rapport à la dernière version des outils Dispatcher.

   ```shell
   $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
   
   ...
   Phase 3: Immutability check
   empty mode param, assuming mode = 'check'
   ...
   ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
   ```

+ Mettez à jour les fichiers non modifiables à l’aide du `update_maven.sh` script

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

+ Vérifiez les fichiers non modifiables mis à jour comme `dispatcher_vhost.conf`, `default.vhost`, et `default.farm` et, si nécessaire, apportez des modifications pertinentes dans vos fichiers personnalisés qui sont dérivés de ces fichiers.

+ Revalidez les configurations, il doit transmettre

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

### docker_run se traduit par un message &quot;En attente jusqu’à ce que host.docker.internal soit disponible&quot;.{#troubleshooting-host-docker-internal}

Le `host.docker.internal` est un nom d’hôte fourni au contenu Docker qui résout l’hôte. Per docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)) :

> À compter de la version 18.03 de Docker, la recommandation consiste à se connecter au nom DNS spécial host.docker.internal, qui correspond à l’adresse IP interne utilisée par l’hôte.

When `bin/docker_run src host.docker.internal:4503 8080` donne le message __En attente jusqu’à ce que host.docker.internal soit disponible__, puis :

1. Assurez-vous que la version installée de Docker est 18.03 ou supérieure.
2. Vous pouvez configurer un ordinateur local qui empêche l’enregistrement/la résolution de la variable `host.docker.internal` nom. Utilisez plutôt votre adresse IP locale.
   + Windows :
   + À partir de l’invite de commande, exécutez `ipconfig`et enregistrez le de l’hôte. __Adresse IPv4__ de la machine hôte.
   + Ensuite, exécutez `docker_run` en utilisant cette adresse IP :
      `bin\docker_run src <HOST IP>:4503 8080`
   + macOS Linux® :
   + Depuis le terminal, exécutez `ifconfig` et enregistrez l’hôte. __inet__ Adresse IP, généralement la variable __en0__ appareil.
   + Exécutez `docker_run` à l’aide de l’adresse IP de l’hôte :
      `bin/docker_run.sh src <HOST IP>:4503 8080`

#### Exemple d’erreur

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Ressources supplémentaires

+ [Télécharger AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Télécharger Docker](https://www.docker.com/)
+ [Téléchargez le site de référence d’AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentation du Dispatcher Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr)
