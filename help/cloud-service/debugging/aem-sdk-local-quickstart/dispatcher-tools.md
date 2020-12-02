---
title: Débogage des outils du répartiteur
description: Les outils du répartiteur fournissent un environnement serveur Web Apache conteneurisé qui peut être utilisé pour simuler AEM en tant que répartiteur local du service de publication AEM Cloud Services. Le débogage des journaux des outils du répartiteur et du contenu du cache peut s’avérer essentiel pour garantir l’exactitude de l’application AEM de bout en bout et des configurations de cache et de sécurité prises en charge.
feature: dispatcher, aem-sdk
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5918
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---


# Débogage des outils du répartiteur

Les outils du répartiteur fournissent un environnement serveur Web Apache conteneurisé qui peut être utilisé pour simuler AEM en tant que répartiteur local du service de publication AEM Cloud Services.
Le débogage des journaux des outils du répartiteur et du contenu du cache peut s’avérer essentiel pour garantir l’exactitude de l’application AEM de bout en bout et des configurations de cache et de sécurité prises en charge.

>[!NOTE]
>
>Comme les outils du répartiteur sont basés sur le conteneur, chaque fois qu&#39;ils sont redémarrés, les journaux antérieurs et le contenu du cache sont détruits.

## Journaux des outils du répartiteur

Les journaux des outils du répartiteur sont disponibles via la commande `stdout` ou `bin/docker_run`, ou avec plus de détails, dans le conteneur Docker à `/etc/https/logs`.

Voir [Journaux du répartiteur](./logs.md#dispatcher-logs) pour obtenir des instructions sur la façon d&#39;accéder directement aux journaux du conteneur Docker des outils du répartiteur.

## Cache des outils du répartiteur

### Accès aux journaux dans le conteneur Docker

Le cache du répartiteur peut être directement accessible dans le conteneur Docker à ` /mnt/var/www/html`.

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

Les journaux du répartiteur peuvent être copiés hors du conteneur Docker à `/mnt/var/www/html` vers le système de fichiers local pour inspection à l&#39;aide de vos outils favoris. Notez qu’il s’agit d’une copie instantanée et qu’elle ne fournit pas de mises à jour en temps réel au cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_merkle

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```

