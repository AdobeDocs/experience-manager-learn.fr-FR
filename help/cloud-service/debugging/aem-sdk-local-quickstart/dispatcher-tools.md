---
title: Déboguer les outils du Dispatcher
description: Les outils du Dispatcher fournissent un environnement de serveur web Apache en conteneur qui peut être utilisé pour simuler localement le Dispatcher du service de publication AEM as a Cloud Service. Le débogage des journaux et du contenu du cache des outils du Dispatcher peut s’avérer essentiel pour s’assurer que l’application AEM de bout en bout et la prise en charge des configurations du cache et de sécurité sont correctes.
feature: Dispatcher
jira: KT-5918
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: f0adf7a6-c7c2-449a-9fa5-402c54b812e5
duration: 60
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 100%

---

# Déboguer les outils du Dispatcher

Les outils du Dispatcher fournissent un environnement de serveur web Apache en conteneur qui peut être utilisé pour simuler localement le Dispatcher du service de publication AEM as a Cloud Service.

Le débogage des journaux et du contenu du cache des outils du Dispatcher peut s’avérer essentiel pour s’assurer que l’application AEM de bout en bout et la prise en charge des configurations du cache et de sécurité sont correctes.

>[!NOTE]
>
>Comme les outils du Dispatcher sont basés sur des conteneurs, les journaux précédents et le contenu du cache sont détruits chaque fois qu’ils sont redémarrés.

## Journaux des outils du Dispatcher

Les journaux des outils du Dispatcher sont disponibles via `stdout` ou la commande `bin/docker_run` ou, avec plus de détails, disponibles dans le conteneur Docker à l’adresse `/etc/https/logs`.

Voir [journaux du Dispatcher](./logs.md#dispatcher-logs) pour obtenir des instructions sur la manière d’accéder directement aux journaux du conteneur Docker des outils du Dispatcher.

## Cache des outils du Dispatcher

### Accéder aux journaux dans le conteneur Docker

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

### Copier les journaux Docker vers le système de fichiers local

Les journaux de Dispatcher peuvent être copiés hors du conteneur Docker à l’adresse `/mnt/var/www/html` vers le système de fichiers local pour l’inspection à l’aide de vos outils préférés. Notez qu’il s’agit d’une copie ponctuelle qui ne fournit pas de mises à jour en temps réel au cache.

```shell
$ docker ps

# locate the CONTAINER ID associated with "adobe/aem-ethos/dispatcher-publisher" IMAGE
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS                  NAMES
46127c9d7081        adobe/aem-ethos/dispatcher-publish:2.0.23   "/docker_entrypoint.…"   6 seconds ago       Up 5 seconds        0.0.0.0:8080->80/tcp   wonderful_ira

$ docker cp -L <CONTAINER ID>:/mnt/var/www/html cache 
$ cd cache
```
