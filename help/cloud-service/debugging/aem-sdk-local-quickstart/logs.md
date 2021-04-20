---
title: Débogage du SDK AEM à l’aide de journaux
description: Les journaux jouent le rôle de première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---


# Débogage du SDK AEM à l’aide de journaux

L’accès aux journaux AEM SDK, soit les Jar de démarrage rapide locale AEM SDK, soit les outils de répartiteur peuvent fournir des informations clés sur le débogage des applications de l’AEM.

## Journaux des AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Les journaux jouent le rôle de première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée. L’Adobe recommande de maintenir le développement local et l’AEM en tant que configurations de journalisation du développement Cloud Service aussi semblables que possible, car il normalise la visibilité du journal sur le démarrage local du SDK AEM et l’ en tant qu’environnements de développement Cloud Service, ce qui réduit le chevauchement et le redéploiement de la configuration.

L&#39;[AEM Archétype de projet](https://github.com/adobe/aem-project-archetype) configure la journalisation au niveau DEBUG pour les packages Java de votre application AEM pour le développement local via la configuration OSGi du Sling Logger, qui se trouve à l&#39;adresse

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

qui se connecte à `error.log`.

Si la journalisation par défaut est insuffisante pour le développement local, la journalisation ad hoc peut être configurée via la console web de prise en charge du journal locale du SDK, à l’adresse ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). Toutefois, les modifications ad hoc recommandées ne sont pas conservées sur Git, sauf si ces mêmes configurations de journalisation sont également nécessaires sur AEM en tant qu’environnements de développement Cloud Service. N’oubliez pas que les modifications effectuées via la console de prise en charge des journaux sont conservées directement dans le référentiel de démarrage rapide local du SDK AEM.

Les instructions du journal Java peuvent être vues dans le fichier `error.log` :

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Souvent, il est utile de &quot;traîner&quot; le `error.log` qui envoie sa sortie au terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiert [des applications tierces](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou l&#39;utilisation de [la commande Get-Content de Powershell](https://stackoverflow.com/a/46444596/133936).

## Journaux du répartiteur

Les journaux du répartiteur sont générés en stdout lorsque `bin/docker_run` est appelé, mais les journaux peuvent être directement accessibles avec dans le contenu du Docker.

### Accès aux journaux dans le conteneur Docker{#dispatcher-tools-access-logs}

Les journaux du répartiteur peuvent être directement accessibles dans le conteneur Docker à `/etc/httpd/logs`.

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

_L&#39; `<CONTAINER ID>` in  `docker exec -it <CONTAINER ID> /bin/sh` doit être remplacé par l&#39;ID de CONTENEUR cible Docker répertorié dans la  `docker ps` commande._


### Copie des journaux Docker vers le système de fichiers local{#dispatcher-tools-copy-logs}

Les journaux du répartiteur peuvent être copiés hors du conteneur Docker à `/etc/httpd/logs` vers le système de fichiers local pour inspection à l&#39;aide de votre outil d&#39;analyse de journal préféré. Notez qu’il s’agit d’une copie ponctuelle et qu’elle ne fournit pas de mises à jour en temps réel des journaux.

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

_L&#39; `<CONTAINER_ID>` in  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` doit être remplacé par l&#39;ID de CONTENEUR cible Docker répertorié dans la  `docker ps` commande._
