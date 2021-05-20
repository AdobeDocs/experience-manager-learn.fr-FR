---
title: Débogage des outils de Dispatcher
description: Les outils de Dispatcher fournissent un environnement de serveur web Apache en conteneur qui peut être utilisé pour simuler localement le Dispatcher du service de publication AEM en tant que Cloud Services. Le débogage des journaux et du contenu du cache des outils de Dispatcher peut s’avérer essentiel pour s’assurer que l’application AEM de bout en bout et la prise en charge des configurations du cache et de sécurité sont correctes.
feature: Dispatcher
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
topic: Développement
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# Débogage des outils de Dispatcher

Les outils de Dispatcher fournissent un environnement de serveur web Apache en conteneur qui peut être utilisé pour simuler localement le Dispatcher du service de publication AEM en tant que Cloud Services.
Le débogage des journaux et du contenu du cache des outils de Dispatcher peut s’avérer essentiel pour s’assurer que l’application AEM de bout en bout et la prise en charge des configurations du cache et de sécurité sont correctes.

>[!NOTE]
>
>Comme les outils Dispatcher sont basés sur le conteneur, chaque fois qu’il est redémarré, les journaux précédents et le contenu du cache sont détruits.

## Journaux des outils Dispatcher

Les journaux des outils de Dispatcher sont disponibles via la commande `stdout` ou `bin/docker_run`, ou avec plus de détails, dans le conteneur Docker à l’adresse `/etc/https/logs`.

Voir [Journaux de Dispatcher](./logs.md#dispatcher-logs) pour obtenir des instructions sur la manière d’accéder directement aux journaux du conteneur Docker des outils de Dispatcher.

## Cache des outils de Dispatcher

### Accès aux journaux dans le conteneur Docker

Le cache de Dispatcher peut être directement accessible dans le conteneur Docker à l’adresse ` /mnt/var/www/html`.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker exec -it <CONTAINER ID> /bin/sh

/ # 
/ # cd /mnt/var/www/html

# When finished viewing the cache, exit the Docker container's shell
/# exit
```

### Copie des journaux Docker vers le système de fichiers local

Les journaux de Dispatcher peuvent être copiés hors du conteneur Docker à l’adresse `/mnt/var/www/html` vers le système de fichiers local pour une inspection à l’aide de vos outils préférés. Notez qu’il s’agit d’une copie ponctuelle qui ne fournit pas de mises à jour en temps réel au cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

