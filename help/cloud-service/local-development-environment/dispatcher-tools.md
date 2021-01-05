---
title: Configuration des outils de répartiteur pour AEM en tant que développement Cloud Service
description: Les outils de répartiteur du SDK AEM facilitent le développement local des projets Adobe Experience Manager (AEM) en facilitant l'installation, l'exécution et la résolution des problèmes du répartiteur localement.
sub-product: fondation
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: 1b4a927a68d24eeb08d0ee244e85519323482910
workflow-type: tm+mt
source-wordcount: '1534'
ht-degree: 2%

---


# Configuration des outils du répartiteur local

Le répartiteur de Adobe Experience Manager (AEM) est un module de serveur Web Apache HTTP qui fournit une couche de sécurité et de performances entre le niveau CDN et le niveau de publication AEM. Le répartiteur fait partie intégrante de l&#39;architecture Experience Manager globale et devrait faire partie du développement local mis en place.

L’AEM en tant que SDK Cloud Service comprend la version recommandée des outils du répartiteur, qui facilite la configuration, la validation et la simulation locale du répartiteur. Les outils du répartiteur se composent des éléments suivants :

+ un ensemble de base de fichiers de configuration du serveur Web Apache HTTP et du répartiteur, situé dans `.../dispatcher-sdk-x.x.x/src`
+ un outil CLI de validation de configuration, situé à `.../dispatcher-sdk-x.x.x/bin/validate` (SDK Dispatcher 2.0.29+)
+ un outil CLI de génération de configuration situé à `.../dispatcher-sdk-x.x.x/bin/validator`
+ un outil d&#39;interface de ligne de commande pour le déploiement de la configuration situé à `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ une image Docker qui exécute le serveur Web HTTP Apache avec le module Répartiteur

Notez que `~` est utilisé comme abrégé pour le Répertoire d&#39;utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`.

>[!NOTE]
>
> Les vidéos de cette page ont été enregistrées sur macOS. Les utilisateurs de Windows peuvent suivre, mais utiliser les commandes Windows équivalentes de Dispatcher Tools, fournies avec chaque vidéo.

## Conditions préalables

1. Les utilisateurs de Windows doivent utiliser Windows 10 Professionnel
1. Installez [Jar de démarrage rapide de la publication Experience Manager](./aem-runtime.md) sur l’ordinateur de développement local.
   + Si vous le souhaitez, installez la dernière version du [site Web de référence ](https://github.com/adobe/aem-guides-wknd/releases) AEM sur le service de publication AEM local. Ce site Web est utilisé dans ce didacticiel pour visualiser un répartiteur fonctionnel.
1. Installez et début la dernière version de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) sur la machine de développement locale.

## Téléchargez les outils du répartiteur (dans le cadre du SDK AEM).

L&#39;AEM en tant que SDK Cloud Service, ou SDK AEM, contient les outils du répartiteur utilisés pour exécuter le serveur Web Apache HTTP avec le module du répartiteur localement pour le développement, ainsi que le JAR QuickStart compatible.

Si l’AEM en tant que SDK Cloud Service a déjà été téléchargé dans [configuration de l’AEM d’exécution locale](./aem-runtime.md), il n’est pas nécessaire de le retélécharger.

1. Connectez-vous à [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout liste&amp;p.offset=0&amp;p.limit=1) avec votre Adobe ID.
   + Votre organisation d’Adobes __doit__ être configurée pour AEM en tant que Cloud Service pour télécharger l’AEM en tant que SDK Cloud Service
1. Cliquez sur la dernière ligne de résultats __AEM SDK__ à télécharger.
   + Assurez-vous que les outils Dispatcher Tools v2.0.29+ du SDK AEM sont indiqués dans la description du téléchargement.

## Extrayez les outils du répartiteur à partir du fichier compressé AEM SDK

>[!TIP]
>
> Les utilisateurs de Windows ne peuvent pas avoir d&#39;espaces ou de caractères spéciaux dans le chemin d&#39;accès au dossier contenant les outils du répartiteur local. Si des espaces existent dans le chemin d’accès, le `docker_run.cmd` échoue.

La version des outils du répartiteur diffère de celle du SDK AEM. Assurez-vous que la version des outils du répartiteur est fournie via la version AEM SDK correspondant à l’AEM en tant que version Cloud Service.

1. Décompresser le fichier `aem-sdk-xxx.zip` téléchargé
1. Décompresser les outils du répartiteur dans `~/aem-sdk/dispatcher`
   + Windows : Décompressez `aem-sdk-dispatcher-tools-x.x.x-windows.zip` dans `C:\Users\<My User>\aem-sdk\dispatcher` (en créant les dossiers manquants si nécessaire).
   + macOS / Linux : Exécutez le script shell `aem-sdk-dispatcher-tools-x.x.x-unix.sh` correspondant pour décompresser les outils du répartiteur.
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Notez que toutes les commandes publiées ci-dessous supposent que le répertoire de travail actuel contient le contenu des outils du répartiteur en expansion.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires*

## Comprendre les fichiers de configuration du répartiteur

>[!TIP]
> Les projets Experience Manager créés à partir de l&#39;[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) sont prérenseignés dans cet ensemble de fichiers de configuration du répartiteur. Il n&#39;est donc pas nécessaire de les copier depuis le dossier src des outils du répartiteur.

Les outils de répartiteur fournissent un ensemble de fichiers de configuration de serveur Web Apache HTTP et de répartiteur qui définissent le comportement de tous les environnements, y compris le développement local.

Ces fichiers sont destinés à être copiés dans un projet Maven Experience Manager vers le dossier `dispatcher/src`, s’ils n’existent pas déjà dans le projet Maven Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires*

Une description complète des fichiers de configuration est disponible dans les outils du répartiteur non compressés sous la forme `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validation des configurations

Si vous le souhaitez, les configurations du Répartiteur et du serveur Web Apache (via `httpd -t`) peuvent être validées à l&#39;aide du script `validate` (à ne pas confondre avec l&#39;exécutable `validator`).

+ Utilisation:
   + Windows : `bin\validate src`
   + macOS / Linux : `./bin/validate ./src`

## Exécuter le répartiteur localement

Pour exécuter le répartiteur localement, les fichiers de configuration du répartiteur doivent être générés à l&#39;aide de l&#39;outil `validator` CLI des outils du répartiteur.

+ Utilisation:
   + Windows : `bin\validator full -d out src`
   + macOS / Linux : `./bin/validator full -d ./out ./src`

Cette commande transforme les configurations en un ensemble de fichiers compatible avec le serveur Web HTTP Apache du conteneur Docker.

Une fois générées, les configurations transposées sont utilisées pour exécuter le répartiteur localement dans le conteneur Docker. Il est important de s&#39;assurer que les dernières configurations ont été validées à l&#39;aide de `validate` __et__ sortie à l&#39;aide de l&#39;option `-d` du validateur.

+ Utilisation:
   + Windows : `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux : `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

`aem-publish-host` peut être défini sur `host.docker.internal`, un nom DNS spécial fourni par Docker dans le conteneur qui correspond à l&#39;adresse IP de l&#39;ordinateur hôte. Si `host.docker.internal` ne résout pas le problème, consultez la section [dépannage](#troubleshooting-host-docker-internal) ci-dessous.

Par exemple, pour début du conteneur du répartiteur Docker à l&#39;aide des fichiers de configuration par défaut fournis par les outils du répartiteur :

1. Générez à partir de zéro le `deployment-folder`, nommé `out` par convention, chaque fois qu&#39;une configuration change :

   + Windows : `del /Q out && bin\validator full -d out src`
   + macOS / Linux : `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. conteneur Docker du répartiteur de débuts (Re-) fournissant le chemin d&#39;accès au dossier de déploiement :

   + Windows : `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux : `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

Le service de publication de l’AEM en tant que SDK Cloud Service s’exécutant localement sur le port 4503 sera disponible par l’intermédiaire de Dispatcher à `http://localhost:8080`.

Pour exécuter les outils du répartiteur par rapport à la configuration du répartiteur d&#39;un projet Experience Manager, générez simplement le dossier `deployment-folder` à l&#39;aide du dossier `dispatcher/src` du projet.

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

## Journaux des outils du répartiteur

Les journaux du répartiteur sont utiles pendant le développement local pour comprendre si et pourquoi les requêtes HTTP sont bloquées. Le niveau de journal peut être défini en préfixant l&#39;exécution de `docker_run` avec des paramètres d&#39;environnement.

Les journaux des outils du répartiteur sont émis vers la sortie standard lorsque `docker_run` est exécuté.

Les paramètres utiles pour le débogage du répartiteur sont les suivants :

+ `DISP_LOG_LEVEL=Debug` définit la journalisation du module Répartiteur au niveau Débogage
   + La valeur par défaut est: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` définit Apache HTTP Web server rewrite module journalisation au niveau Debug
   + La valeur par défaut est: `Warn`
+ `DISP_RUN_MODE` définit le &quot;mode d&#39;exécution&quot; de l&#39;environnement Répartiteur, en chargeant les fichiers de configuration Répartiteur des modes d&#39;exécution correspondants.
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

## Quand mettre à jour les outils du répartiteur{#dispatcher-tools-version}

Les versions des outils du répartiteur s’incrémentent moins fréquemment que le Experience Manager et les outils du répartiteur nécessitent donc moins de mises à jour dans l’environnement de développement local.

La version des outils du répartiteur recommandée est celle qui est fournie avec l’AEM en tant que SDK Cloud Service et qui correspond au Experience Manager en tant que version Cloud Service. La version de l’AEM en tant que Cloud Service peut être consultée via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Environnements__, par environnement spécifié par le  __AEM__ Releaselabel

![Version du Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Notez que la version des outils du répartiteur elle-même ne correspond pas à la version du Experience Manager._

## Résolution des incidents

### docker_run se traduit par un message &quot;En attente jusqu&#39;à ce que host.docker.internal soit disponible&quot;{#troubleshooting-host-docker-internal}

`host.docker.internal` est un nom d&#39;hôte fourni au Docker contient qui correspond à l&#39;hôte. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)) :

> À partir de la version 18.03 de Docker, nous vous recommandons de vous connecter au nom DNS spécial host.docker.internal, qui correspond à l&#39;adresse IP interne utilisée par l&#39;hôte.

Si, lorsque `bin/docker_run out host.docker.internal:4503 8080` renvoie le message __En attendant que host.docker.internal soit disponible__, alors :

1. Assurez-vous que la version installée de Docker est 18.03 ou supérieure.
2. Vous pouvez configurer un ordinateur local qui empêche l&#39;enregistrement/la résolution du nom `host.docker.internal`. Utilisez plutôt votre adresse IP locale.
   + Windows :
      + Dans l&#39;invite de commandes, exécutez `ipconfig` et enregistrez l&#39;adresse __IPv4__ de l&#39;hôte sur l&#39;ordinateur hôte.
      + Exécutez ensuite `docker_run` en utilisant cette adresse IP :
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux :
      + Depuis le terminal, exécutez `ifconfig` et enregistrez l&#39;adresse IP de l&#39;hôte __inet__, généralement le périphérique __en0__.
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

### docker_run génère l&#39;erreur &quot;**&quot; : Dossier de déploiement introuvable

Lors de l&#39;exécution de `docker_run.cmd`, une erreur s&#39;affiche et indique l&#39;erreur __** : Dossier de déploiement introuvable :__. Cela se produit généralement parce qu’il y a des espaces dans le chemin d’accès. Si possible, supprimez les espaces du dossier ou déplacez le dossier `aem-sdk` vers un chemin qui ne contient pas d&#39;espaces.

Par exemple, les dossiers utilisateur Windows sont souvent `<First name> <Last name>`, avec un espace entre les deux. Dans l&#39;exemple ci-dessous, le dossier `...\My User\...` contient un espace qui rompt l&#39;exécution `docker_run` des outils du répartiteur local. Si les espaces se trouvent dans un dossier d&#39;utilisateur Windows, n&#39;essayez pas de renommer ce dossier car il va rompre Windows. Déplacez plutôt le dossier `aem-sdk` vers un nouvel emplacement que votre utilisateur est autorisé à modifier complètement. Notez que les instructions qui supposent que le dossier `aem-sdk` se trouve dans le répertoire d’accueil de l’utilisateur devront être ajustées au nouvel emplacement.

#### Exemple d’erreur

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### échec du début de docker_run sous Windows{#troubleshooting-windows-compatible}

L&#39;exécution de `docker_run` sous Windows peut générer l&#39;erreur suivante, empêchant le démarrage du répartiteur. Il s’agit d’un problème signalé avec Dispatcher sous Windows et qui sera corrigé dans une prochaine version.

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
+ [Télécharger le Docteur](https://www.docker.com/)
+ [Télécharger le site Web de référence AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentation du répartiteur Experience Manager](https://docs.adobe.com/content/help/fr-FR/experience-manager-dispatcher/using/dispatcher.html)
