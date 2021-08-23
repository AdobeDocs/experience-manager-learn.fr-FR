---
title: Configuration des outils Dispatcher pour AEM en tant que développement de Cloud Service
description: Les outils Dispatcher du SDK AEM facilitent le développement local de projets Adobe Experience Manager (AEM) en facilitant l’installation, l’exécution et le dépannage local de Dispatcher.
sub-product: foundation
feature: Dispatcher, outils de développement
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1637'
ht-degree: 2%

---


# Configuration des outils Dispatcher locaux

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Outils du Dispatcher local"
>abstract="Dispatcher fait partie intégrante de l’architecture globale du Experience Manager et doit faire partie de la configuration du développement local. Le SDK AEM as a Cloud Service comprend la version recommandée des outils Dispatcher, qui facilite la configuration, la validation et la simulation locale de Dispatcher."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher en mode cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Téléchargement d’AEM en tant que SDK Cloud Service"

Dispatcher d’Adobe Experience Manager (AEM) est un module de serveur web Apache HTTP qui fournit une couche de sécurité et de performances entre le réseau de diffusion de contenu et le niveau Publication AEM. Dispatcher fait partie intégrante de l’architecture globale du Experience Manager et doit faire partie de la configuration du développement local.

Le SDK AEM as a Cloud Service comprend la version recommandée des outils Dispatcher, qui facilite la configuration, la validation et la simulation locale de Dispatcher. Les outils de Dispatcher se composent des éléments suivants :

+ un ensemble de base de fichiers de configuration du serveur web Apache HTTP et du Dispatcher, situé dans `.../dispatcher-sdk-x.x.x/src`
+ un outil d’interface de ligne de commande du validateur de configuration, situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/validate` (SDK Dispatcher 2.0.29+).
+ un outil d’interface de ligne de commande pour la génération de la configuration situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/validator`
+ un outil d’interface de ligne de commande pour le déploiement des configurations situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ une image Docker qui exécute le serveur web Apache HTTP avec le module Dispatcher ;

Notez que `~` est utilisé comme abrégé pour le répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

>[!NOTE]
>
> Les vidéos de cette page ont été enregistrées sur macOS. Les utilisateurs de Windows peuvent suivre, mais utiliser les commandes Windows équivalentes des outils Dispatcher, fournies avec chaque vidéo.

## Prérequis

1. Les utilisateurs de Windows doivent utiliser Windows 10 Professional
1. Installez [Experience Manager Publish Quickstart Jar](./aem-runtime.md) sur la machine de développement locale.
   + Vous pouvez éventuellement installer le dernier [site web de référence d’AEM](https://github.com/adobe/aem-guides-wknd/releases) sur le service AEM Publish local. Ce site web est utilisé dans ce tutoriel pour visualiser un Dispatcher fonctionnel.
1. Installez et démarrez la dernière version de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+/Docker Engine v19.03.9+) sur l’ordinateur de développement local.

## Téléchargement des outils de Dispatcher (dans le cadre du SDK AEM)

Le SDK AEM as a Cloud Service, ou SDK AEM, contient les outils Dispatcher utilisés pour exécuter le serveur web Apache HTTP avec le module Dispatcher localement pour le développement, ainsi que le fichier Jar QuickStart compatible.

Si le SDK AEM as a Cloud Service a déjà été téléchargé vers [configuration du runtime AEM local](./aem-runtime.md), il n’est pas nécessaire de le retélécharger.

1. Connectez-vous à [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=1) avec votre Adobe ID
   + Votre organisation Adobe __doit__ être configurée pour AEM en tant que Cloud Service pour télécharger AEM en tant que SDK Cloud Service
1. Cliquez sur la dernière __ligne de résultat AEM SDK__ à télécharger.
   + Assurez-vous que les outils Dispatcher du SDK AEM v2.0.29+ sont indiqués dans la description du téléchargement.

## Extrayez les outils Dispatcher du fichier zip du SDK AEM

>[!TIP]
>
> Les utilisateurs de Windows ne peuvent pas avoir d’espaces ni de caractères spéciaux dans le chemin d’accès au dossier contenant les outils du Dispatcher local. S’il existe des espaces dans le chemin, la balise `docker_run.cmd` échoue.

La version des outils de Dispatcher est différente de celle du SDK AEM. Assurez-vous que la version des outils de Dispatcher est fournie via la version AEM SDK correspondant à l’AEM en tant que version de Cloud Service.

1. Décompressez le fichier `aem-sdk-xxx.zip` téléchargé.
1. Décompressez les outils Dispatcher dans `~/aem-sdk/dispatcher`
   + Windows : Décompressez `aem-sdk-dispatcher-tools-x.x.x-windows.zip` dans `C:\Users\<My User>\aem-sdk\dispatcher` (créez les dossiers manquants selon les besoins).
   + macOS / Linux : Exécutez le script shell suivant `aem-sdk-dispatcher-tools-x.x.x-unix.sh` pour décompresser les outils de Dispatcher.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Notez que toutes les commandes émises ci-dessous supposent que le répertoire de travail actuel contient le contenu des outils Dispatcher en extension.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires*

## Présentation des fichiers de configuration de Dispatcher

>[!TIP]
> Les projets de Experience Manager créés à partir de l’[archétype Maven de projet AEM](https://github.com/adobe/aem-project-archetype) sont prérenseignés à cet ensemble de fichiers de configuration de Dispatcher. Il n’est donc pas nécessaire de les copier depuis le dossier src des outils de Dispatcher.

Les outils de Dispatcher fournissent un ensemble de fichiers de configuration du serveur web Apache HTTP et de Dispatcher qui définissent le comportement de tous les environnements, y compris le développement local.

Ces fichiers sont destinés à être copiés dans un projet Maven Experience Manager vers le dossier `dispatcher/src`, s’ils n’existent pas déjà dans le projet Maven Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires*

Une description complète des fichiers de configuration est disponible dans les outils Dispatcher décompressés sous la forme `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validation des configurations

En option, les configurations du serveur web Dispatcher et Apache (via `httpd -t`) peuvent être validées à l’aide du script `validate` (à ne pas confondre avec le fichier exécutable `validator`).

+ Utilisation:
   + Windows : `bin\validate src`
   + macOS / Linux : `./bin/validate.sh ./src`

## Exécution locale de Dispatcher

Pour exécuter Dispatcher localement, les fichiers de configuration de Dispatcher doivent être générés à l’aide de l’outil d’interface de ligne de commande `validator` de Dispatcher.

+ Utilisation:
   + Windows : `bin\validator full -d out src`
   + macOS / Linux : `./bin/validator full -d ./out ./src`

Cette commande transforme les configurations en un ensemble de fichiers compatible avec le serveur web Apache HTTP du conteneur Docker.

Une fois générées, les configurations transposées sont utilisées pour exécuter Dispatcher localement dans le conteneur Docker. Il est important de s’assurer que les dernières configurations ont été validées à l’aide des sorties `validate` __et__ à l’aide de l’option `-d` du programme de validation.

+ Utilisation:
   + Windows : `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux : `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host` peut être défini sur `host.docker.internal`, un nom DNS spécial que Docker fournit dans le conteneur qui résout l’adresse IP de l’ordinateur hôte. Si la `host.docker.internal` ne se résout pas, reportez-vous à la section [Dépannage](#troubleshooting-host-docker-internal) ci-dessous.

Par exemple, pour démarrer le conteneur Docker de Dispatcher à l’aide des fichiers de configuration par défaut fournis par les outils Dispatcher :

1. Générez à partir de zéro la `deployment-folder`, nommée `out` par convention, chaque fois qu’une configuration change :

   + Windows : `del /Q out && bin\validator full -d out src`
   + macOS / Linux : `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Redémarrez) le conteneur Docker de Dispatcher en indiquant le chemin d’accès au dossier de déploiement :

   + Windows : `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux : `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Le service de publication de l’AEM as a Cloud Service SDK s’exécutant localement sur le port 4503 sera disponible via Dispatcher à l’adresse `http://localhost:8080`.

Pour exécuter les outils Dispatcher par rapport à la configuration du Dispatcher d’un projet de Experience Manager, il vous suffit de générer la balise `deployment-folder` à l’aide du dossier `dispatcher/src` du projet.

+ Windows :

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux :

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires*

## Journaux des outils Dispatcher

Les journaux de Dispatcher sont utiles lors du développement local pour comprendre si et pourquoi les requêtes HTTP sont bloquées. Le niveau de journal peut être défini en ajoutant un préfixe à l’exécution de `docker_run` avec les paramètres d’environnement.

Les journaux des outils de Dispatcher sont émis en sortie standard lorsque `docker_run` est exécuté.

Les paramètres utiles pour le débogage de Dispatcher incluent :

+ `DISP_LOG_LEVEL=Debug` définit la journalisation du module Dispatcher sur le niveau de débogage.
   + La valeur par défaut est: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` définit la journalisation du module de réécriture du serveur web Apache HTTP au niveau de débogage.
   + La valeur par défaut est: `Warn`
+ `DISP_RUN_MODE` définit le &quot;mode d’exécution&quot; de l’environnement de Dispatcher, en chargeant les fichiers de configuration de Dispatcher des modes d’exécution correspondants.
   + La valeur par défaut est `dev`
+ Valeurs valides : `dev`, `stage` ou `prod`

Un ou plusieurs paramètres peuvent être transmis à `docker_run`

+ Windows :

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux :

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires*

### Accès aux fichiers journaux

Le serveur web Apache et les journaux AEM Dispatcher sont directement accessibles dans le conteneur Docker :

+ [Accès aux journaux dans le conteneur Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copie des journaux Docker vers le système de fichiers local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quand mettre à jour les outils de Dispatcher{#dispatcher-tools-version}

Les versions des outils de Dispatcher s’incrémentent moins fréquemment que le Experience Manager. Par conséquent, les outils de Dispatcher nécessitent moins de mises à jour dans l’environnement de développement local.

La version recommandée des outils de Dispatcher est celle qui est fournie avec l’AEM en tant que SDK de Cloud Service correspondant au Experience Manager en tant que version de Cloud Service. Vous trouverez la version d’AEM en tant que Cloud Service via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Environnements__, par environnement spécifié par le  ____ Libellé d’AEM

![Version du Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Notez que la version des outils de Dispatcher elle-même ne correspond pas à la version du Experience Manager._

## Résolution des problèmes

### docker_run se traduit par un message &quot;En attente jusqu’à ce que host.docker.internal soit disponible&quot;.{#troubleshooting-host-docker-internal}

`host.docker.internal` est un nom d’hôte fourni au contenu Docker qui résout l’hôte. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)) :

> À partir de la version 18.03 de Docker, nous vous recommandons de vous connecter au nom DNS spécial host.docker.internal, qui correspond à l’adresse IP interne utilisée par l’hôte.

Si, lorsque `bin/docker_run out host.docker.internal:4503 8080` renvoie le message __En attendant que host.docker.internal soit disponible__, alors :

1. Assurez-vous que la version installée de Docker est 18.03 ou supérieure.
2. Vous pouvez configurer un ordinateur local qui empêche l’enregistrement/la résolution du nom `host.docker.internal`. Utilisez plutôt votre adresse IP locale.
   + Windows :
      + Dans l’invite de commande, exécutez `ipconfig` et enregistrez l’__adresse IPv4__ de l’ordinateur hôte.
      + Exécutez ensuite `docker_run` à l’aide de cette adresse IP :
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux :
      + Depuis le terminal, exécutez `ifconfig` et enregistrez l’adresse IP de l’hôte __inet__, généralement le périphérique __en0__.
      + Exécutez ensuite `docker_run` à l’aide de l’adresse IP de l’hôte :
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Exemple d’erreur

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run génère une erreur ** : Dossier de déploiement introuvable&quot;

Lors de l’exécution de `docker_run.cmd`, une erreur s’affiche et indique __** erreur : Dossier de déploiement introuvable :__. Cela se produit généralement car le chemin contient des espaces. Si possible, supprimez les espaces dans le dossier ou déplacez le dossier `aem-sdk` vers un chemin ne contenant pas d’espaces.

Par exemple, les dossiers d’utilisateurs Windows sont souvent `<First name> <Last name>`, avec un espace entre eux. Dans l’exemple ci-dessous, le dossier `...\My User\...` contient un espace qui interrompt l’exécution `docker_run` locale des outils Dispatcher. Si les espaces se trouvent dans un dossier d’utilisateurs Windows, ne tentez pas de renommer ce dossier, car il va rompre Windows. Déplacez plutôt le dossier `aem-sdk` vers un nouvel emplacement que votre utilisateur est autorisé à modifier. Notez que les instructions qui supposent que le dossier `aem-sdk` se trouve dans le répertoire racine de l’utilisateur devront être ajustées au nouvel emplacement.

#### Exemple d’erreur

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### échec du démarrage de docker_run sous Windows{#troubleshooting-windows-compatible}

L’exécution de `docker_run` sous Windows peut entraîner l’erreur suivante, ce qui empêche Dispatcher de démarrer. Il s’agit d’un problème signalé avec Dispatcher sous Windows qui sera corrigé dans une version ultérieure.

#### Exemple d’erreur

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## Ressources supplémentaires

+ [Télécharger AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Télécharger Docker](https://www.docker.com/)
+ [Téléchargez le site de référence d’AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentation du Dispatcher Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr)
