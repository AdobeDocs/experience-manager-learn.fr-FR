---
title: Débogage AEM SDK à l’aide des journaux
description: Les journaux jouent le rôle de première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée.
feature: Outils de développement
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5252
topic: Développement
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 3%

---


# Débogage AEM SDK à l’aide des journaux

L’accès aux journaux du SDK AEM, que ce soit les fichiers Jar de démarrage rapide locaux du SDK AEM ou les outils de Dispatcher peuvent fournir des informations clés sur le débogage d’applications d’AEM.

## Journaux AEM

>[!VIDEO](https://video.tv.adobe.com/v/34334/?quality=12&learn=on)

Les journaux jouent le rôle de première ligne pour le débogage des applications AEM, mais dépendent d’une journalisation adéquate dans l’application AEM déployée. Adobe recommande de conserver les configurations de journalisation de développement local et d’AEM en tant que Cloud Service aussi similaires que possible, car cela normalise la visibilité du journal sur le démarrage rapide local du SDK AEM et l’ en tant qu’environnements de développement Cloud Service, ce qui réduit le délai de configuration et le redéploiement.

L’ [archétype de projet AEM](https://github.com/adobe/aem-project-archetype) configure la journalisation au niveau DEBUG pour les packages Java de votre application AEM pour le développement local via la configuration OSGi de l’enregistreur Sling située à l’adresse

`ui.apps/src/main/content/jcr_root/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

qui se connecte à `error.log`.

Si la journalisation par défaut n’est pas suffisante pour le développement local, la journalisation ad hoc peut être configurée via la console web de prise en charge des journaux du SDK local, à l’adresse ([/system/console/slinglog](http://localhost:4502/system/console/slinglog)). Toutefois, les modifications ad hoc recommandées ne sont pas conservées dans Git, sauf si ces mêmes configurations de journal sont également nécessaires dans AEM en tant qu’environnements de développement Cloud Service. Gardez à l’esprit que les modifications effectuées via la console Prise en charge du journal sont conservées directement dans le référentiel de démarrage rapide local du SDK AEM.

Les instructions du journal Java peuvent être visualisées dans le fichier `error.log` :

```
$ ~/aem-sdk/author/crx-quickstart/logs/error.log
```

Il est souvent utile de &quot;tail&quot; la `error.log` qui diffuse sa sortie vers le terminal.

+ macOS/Linux
   + `$ tail -f ~/aem-sdk/author/crx-quickstart/logs/error.log`
+ Windows requiert [des applications tierces](https://stackoverflow.com/questions/187587/a-windows-equivalent-of-the-unix-tail-command) ou l’utilisation de la [commande Get-Content de PowerShell](https://stackoverflow.com/a/46444596/133936).

## Journaux de Dispatcher

Les journaux de Dispatcher sont sortis vers stdout lorsque `bin/docker_run` est appelé, mais les journaux peuvent être directement accessibles avec dans le contenu Docker.

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

_Le  `<CONTAINER ID>` dans  `docker exec -it <CONTAINER ID> /bin/sh` doit être remplacé par l’identifiant Docker CONTAINER cible répertorié dans la  `docker ps` commande ._


### Copie des journaux Docker vers le système de fichiers local{#dispatcher-tools-copy-logs}

Les journaux de Dispatcher peuvent être copiés hors du conteneur Docker à l’adresse `/etc/httpd/logs` vers le système de fichiers local pour inspection à l’aide de votre outil d’analyse de journal favori. Notez qu’il s’agit d’une copie ponctuelle qui ne fournit pas de mises à jour en temps réel des journaux.

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

_Le  `<CONTAINER_ID>` dans  `docker cp <CONTAINER_ID>:/var/log/apache2 ./` doit être remplacé par l’identifiant Docker CONTAINER cible répertorié dans la  `docker ps` commande ._
