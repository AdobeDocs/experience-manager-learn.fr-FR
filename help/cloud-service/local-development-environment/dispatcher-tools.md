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
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 3%

---

# Configuration des outils Dispatcher locaux {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Outils du Dispatcher local"
>abstract="Dispatcher fait partie intégrante de l’architecture globale du Experience Manager et doit faire partie de la configuration du développement local. Le SDK as a Cloud Service d’AEM comprend la version recommandée des outils Dispatcher, qui facilite la configuration, la validation et la simulation locale de Dispatcher."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html?lang=fr" text="Dispatcher en mode cloud"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Télécharger AEM SDK as a Cloud Service"

Dispatcher d’Adobe Experience Manager (AEM) est un module de serveur web Apache HTTP qui fournit une couche de sécurité et de performances entre le réseau de diffusion de contenu et le niveau Publication AEM. Dispatcher fait partie intégrante de l’architecture globale du Experience Manager et doit faire partie de la configuration du développement local.

Le SDK as a Cloud Service d’AEM comprend la version recommandée des outils Dispatcher, qui facilite la configuration, la validation et la simulation locale de Dispatcher. Les outils de Dispatcher se composent des éléments suivants :

+ un ensemble de base de fichiers de configuration du serveur web Apache HTTP et du Dispatcher, situé dans `.../dispatcher-sdk-x.x.x/src`
+ un outil d’interface de ligne de commande du validateur de configuration situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/validate`
+ un outil d’interface de ligne de commande de génération de configuration situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/validator`
+ un outil d’interface de ligne de commande pour le déploiement des configurations situé à l’adresse `.../dispatcher-sdk-x.x.x/bin/docker_run`
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

Le SDK as a Cloud Service AEM, ou SDK AEM, contient les outils Dispatcher utilisés pour exécuter le serveur web Apache HTTP avec le module Dispatcher localement pour le développement, ainsi que le fichier Jar QuickStart compatible.

Si le SDK as a Cloud Service AEM a déjà été téléchargé vers [configuration de l’exécution d’AEM locale](./aem-runtime.md), il n’est pas nécessaire de le retélécharger.

1. Connectez-vous à [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=1) avec votre Adobe ID
   + Votre organisation Adobe __must__ être configuré pour AEM as a Cloud Service pour télécharger AEM SDK as a Cloud Service
1. Cliquez sur la dernière __AEM SDK__ ligne de résultat à télécharger

## Extrayez les outils Dispatcher du fichier zip du SDK AEM

>[!TIP]
>
> Les utilisateurs de Windows ne peuvent pas avoir d’espaces ni de caractères spéciaux dans le chemin d’accès au dossier contenant les outils du Dispatcher local. S’il existe des espaces dans le chemin, la variable `docker_run.cmd` échouera.

La version des outils de Dispatcher est différente de celle du SDK AEM. Assurez-vous que la version des outils de Dispatcher est fournie via la version AEM SDK correspondant à la version AEM as a Cloud Service.

1. Décompressez le fichier téléchargé. `aem-sdk-xxx.zip` fichier
1. Décompressez les outils Dispatcher dans `~/aem-sdk/dispatcher`
   + Windows : Décompresser `aem-sdk-dispatcher-tools-x.x.x-windows.zip` into `C:\Users\<My User>\aem-sdk\dispatcher` (création de dossiers manquants, si nécessaire)
   + macOS / Linux : Exécution du script shell associé `aem-sdk-dispatcher-tools-x.x.x-unix.sh` pour décompresser les outils de Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Notez que toutes les commandes émises ci-dessous supposent que le répertoire de travail actuel contient le contenu des outils Dispatcher en extension.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Cette vidéo utilise macOS à des fins d’illustration. Les commandes Windows/Linux équivalentes peuvent être utilisées pour obtenir des résultats similaires.*

## Présentation des fichiers de configuration de Dispatcher

>[!TIP]
> Projets Experience Manager créés à partir de [Archétype Maven de projet AEM](https://github.com/adobe/aem-project-archetype) sont prérenseignés à cet ensemble de fichiers de configuration de Dispatcher. Il n’est donc pas nécessaire de les copier depuis le dossier src des outils Dispatcher.

Les outils de Dispatcher fournissent un ensemble de fichiers de configuration du serveur web Apache HTTP et de Dispatcher qui définissent le comportement de tous les environnements, y compris le développement local.

Ces fichiers sont destinés à être copiés dans un projet Maven Experience Manager vers le `dispatcher/src` s’ils n’existent pas déjà dans le projet Maven Experience Manager.

Une description complète des fichiers de configuration est disponible dans les outils Dispatcher décompressés sous la forme `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validation des configurations

Facultativement, les configurations du serveur web Dispatcher et Apache (via `httpd -t`) peut être validé à l’aide de la variable `validate` (à ne pas confondre avec le `validator` exécutable). Le `validate` Le script offre un moyen pratique d’exécuter la variable [3 phases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) de `validator`.

+ Utilisation:
   + Windows : `bin\validate src`
   + macOS / Linux : `./bin/validate.sh ./src`

## Exécution locale de Dispatcher

AEM Dispatcher est exécuté localement à l’aide de Docker par rapport à `src` Fichiers de configuration de Dispatcher et du serveur web Apache.

+ Utilisation:
   + Windows : `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux : `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

Le `<aem-publish-host>` peut être défini sur `host.docker.internal`, un nom DNS spécial que Docker fournit dans le conteneur qui résout l’adresse IP de l’ordinateur hôte. Si `host.docker.internal` ne résout pas, reportez-vous à la section [dépannage](#troubleshooting-host-docker-internal) ci-dessous.

Par exemple, pour démarrer le conteneur Docker de Dispatcher à l’aide des fichiers de configuration par défaut fournis par les outils Dispatcher :

Démarrez le conteneur Docker de Dispatcher en indiquant le chemin d’accès au dossier src de configuration de Dispatcher :

+ Windows : `bin\docker_run src host.docker.internal:4503 8080`
+ macOS / Linux : `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

Le service de publication du SDK as a Cloud Service, s’exécutant localement sur le port 4503, est disponible via Dispatcher à l’adresse `http://localhost:8080`.

Pour exécuter les outils Dispatcher par rapport à la configuration Dispatcher d’un projet de Experience Manager, pointez sur la variable `dispatcher/src` dossier.

+ Windows :

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux :

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
   + La valeur par défaut est `dev`
+ Valeurs valides : `dev`, `stage`ou `prod`

Un ou plusieurs paramètres peuvent être transmis à `docker_run`

+ Windows :

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux :

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

_Notez que la version des outils de Dispatcher elle-même ne correspond pas à la version du Experience Manager._

## Résolution des problèmes

### docker_run se traduit par un message &quot;En attente jusqu’à ce que host.docker.internal soit disponible&quot;.{#troubleshooting-host-docker-internal}

`host.docker.internal` est un nom d’hôte fourni au contenu Docker qui résout l’hôte. Per docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)) :

> À partir de la version 18.03 de Docker, nous vous recommandons de vous connecter au nom DNS spécial host.docker.internal, qui correspond à l’adresse IP interne utilisée par l’hôte.

Si, lorsque `bin/docker_run src host.docker.internal:4503 8080` donne le message __En attente jusqu’à ce que host.docker.internal soit disponible__, puis :

1. Assurez-vous que la version installée de Docker est 18.03 ou supérieure.
2. Vous pouvez configurer un ordinateur local qui empêche l’enregistrement/la résolution de la variable `host.docker.internal` nom. Utilisez plutôt votre adresse IP locale.
   + Windows :
      + À partir de l’invite de commande, exécutez `ipconfig`et enregistrez le de l’hôte. __Adresse IPv4__ de la machine hôte.
      + Ensuite, exécutez `docker_run` en utilisant cette adresse IP :
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS / Linux :
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
