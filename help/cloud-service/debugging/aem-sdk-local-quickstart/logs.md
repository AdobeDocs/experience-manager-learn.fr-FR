---
title: Débogage AEM SDK à l’aide des journaux
description: Les journaux jouent le rôle de première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 91aa4a10-47fe-4313-acd2-ca753e5484d9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---

# Débogage AEM SDK à l’aide des journaux

L’accès aux journaux du SDK AEM, que ce soit les fichiers Jar de démarrage rapide locaux du SDK AEM ou les outils de Dispatcher peuvent fournir des informations clés sur le débogage d’applications d’AEM.

## Journaux AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334?quality=12&learn=on)

Les journaux jouent le rôle de première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée. Adobe recommande de conserver autant que possible les configurations de journalisation de développement local et de développement as a Cloud Service, car cela normalise la visibilité du journal sur le démarrage rapide local du SDK AEM et les environnements de développement d’as a Cloud Service, ce qui réduit le délai de configuration et le redéploiement.

Le [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype) configure la journalisation au niveau DEBUG pour les packages Java de votre application AEM pour le développement local via la configuration OSGi Sling Logger qui se trouve à l’adresse

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

qui se connecte au `error.log`.

Si la journalisation par défaut n’est pas suffisante pour le développement local, la journalisation ad hoc peut être configurée via la console web de prise en charge des journaux locale du SDK AEM, à l’adresse ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)), toutefois, il n’est pas recommandé que les modifications ad hoc soient conservées dans Git, sauf si ces mêmes configurations de journal sont également nécessaires dans AEM environnements de développement as a Cloud Service. Gardez à l’esprit que les modifications effectuées via la console Prise en charge du journal sont conservées directement dans le référentiel de démarrage rapide local du SDK AEM.

Les instructions du journal Java peuvent être visualisées dans la variable `error.log` fichier :

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Souvent, il est utile de &quot;tail&quot; la `error.log` qui diffuse sa sortie vers le terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows nécessite [Applications tierces](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou l’utilisation de [Commande Get-Content de PowerShell](https://stackoverflow.com/a/46444596/133936).

## Journaux de Dispatcher

Les journaux de Dispatcher sont sortis en mode stdout lorsque `bin/docker_run` est appelée, mais les journaux peuvent être directement accessibles avec dans le contenu Docker.

### Accès aux journaux dans le conteneur Docker{#dispatcher-tools-access-logs}

Les journaux de Dispatcher peuvent accéder directement dans le conteneur Docker à l’adresse `/etc/httpd/logs`.

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

_Le `<CONTAINER ID>` in `docker exec -it <CONTAINER ID> /bin/sh` doit être remplacé par l’ID de CONTENEUR Docker cible répertorié dans la variable `docker ps` ._


### Copie des journaux Docker vers le système de fichiers local{#dispatcher-tools-copy-logs}

Les journaux de Dispatcher peuvent être copiés hors du conteneur Docker à l’adresse `/etc/httpd/logs` au système de fichiers local pour l’inspection à l’aide de votre outil d’analyse de journal préféré. Notez qu’il s’agit d’une copie ponctuelle qui ne fournit pas de mises à jour en temps réel des journaux.

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

_Le `<CONTAINER_ID>` in `docker cp <CONTAINER_ID>:/var/log/apache2 ./` doit être remplacé par l’ID de CONTENEUR Docker cible répertorié dans la variable `docker ps` ._
