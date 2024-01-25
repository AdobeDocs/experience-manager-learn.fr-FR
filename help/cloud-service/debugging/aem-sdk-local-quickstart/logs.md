---
title: Déboguer le SDK d’AEM à l’aide de journaux
description: Les journaux sont en première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
duration: 430
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 100%

---

# Déboguer le SDK d’AEM à l’aide de journaux

Les journaux du SDK AEM, qu’il s’agisse des fichiers Jar de démarrage rapide locaux du SDK AEM ou des outils du Dispatcher, peuvent fournir des informations clés sur le débogage des applications AEM.

## Journaux AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Les journaux sont en première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée. Adobe recommande, dans la mesure du possible, de ne pas modifier la configuration de la journalisation pour l’environnement de développement local et l’environnement de développement AEM as a Cloud Service, car cela permet de normaliser la visibilité de la journalisation dans les environnements locaux de démarrage rapide du SDK AEM et de développement AEM as a Cloud Service, ce qui réduit le temps de configuration et de redéploiement.

L’[archétype de projet AEM](https://github.com/adobe/aem-project-archetype) permet de configurer la journalisation au niveau DEBUG pour les packages Java de votre application AEM pour le développement local via la configuration OSGi des journaux Sling, qui se trouve à l’emplacement suivant :

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

Elle consigne les informations dans le fichier `error.log`.

Si la journalisation par défaut n’est pas suffisante pour le développement local, la journalisation ad hoc peut être configurée via la console web de prise en charge des journaux du démarrage rapide local du SDK AEM, à l’emplacement ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). Il n’est toutefois pas recommandé que les modifications apportées soient conservées dans Git, sauf si ces mêmes configurations de journal sont également nécessaires dans les environnements de développement AEM as a Cloud Service. Gardez à l’esprit que les modifications effectuées via la console de prise en charge de la journalisation sont conservées directement dans le référentiel de démarrage rapide local du SDK AEM.

Les instructions du journal Java peuvent être visualisées dans le fichier `error.log` :

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Souvent, il est utile d’analyser le `error.log` qui diffuse sa sortie vers le terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows nécessite des [applications d’analyse tierces](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou l’utilisation d’une [commande Get-Content de PowerShell](https://stackoverflow.com/a/46444596/133936).

## Journaux de Dispatcher

Les journaux de Dispatcher sont envoyés vers stdout lorsque `bin/docker_run` est appelé, mais les journaux peuvent être directement accessibles dans le conteneur Docker.

### Accéder aux journaux dans le conteneur Docker{#dispatcher-tools-access-logs}

Il est possible d’accéder directement aux journaux de Dispatcher dans le conteneur Docker à l’emplacement `/etc/httpd/logs`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /etc/httpd/logs
/ # ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log

# When finished viewing the logs files, exit the Docker container's shell
/# exit
```

_L’`<CONTAINER ID>` dans `docker exec -it <CONTAINER ID> /bin/sh` doit être remplacé par l’ID de conteneur Docker cible répertorié dans la commande `docker ps`._


### Copier les journaux Docker vers le système de fichiers local{#dispatcher-tools-copy-logs}

Les journaux de Dispatcher peuvent être copiés du conteneur Docker à l’emplacement `/etc/httpd/logs` vers le système de fichiers local pour inspection à l’aide de votre outil d’analyse de journal préféré. Notez qu’il s’agit d’une copie ponctuelle qui ne fournit pas de mises à jour en temps réel des journaux.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/etc/httpd/logs logs 
$ cd logs
$ ls
    dispatcher.log          healthcheck_access_log  httpd_access.log        httpd_error.log
```

_L’`<CONTAINER_ID>` dans `docker cp <CONTAINER_ID>:/var/log/apache2 ./` doit être remplacé par l’ID de conteneur Docker cible répertorié dans la commande `docker ps`._
