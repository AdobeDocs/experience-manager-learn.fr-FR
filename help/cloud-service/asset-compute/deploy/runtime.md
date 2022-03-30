---
title: Déployer des Assets compute Worker dans Adobe I/O Runtime pour une utilisation avec AEM as a Cloud Service
description: Les projets d’Asset compute, et les travailleurs qu’ils contiennent, doivent être déployés sur Adobe I/O Runtime pour être utilisés par AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Déploiement sur Adobe I/O Runtime

Les projets d’Asset compute, ainsi que les travailleurs qu’ils contiennent, doivent être déployés vers Adobe I/O Runtime via l’interface de ligne de commande de l’Adobe I/O pour être utilisés par AEM as a Cloud Service.

Lors d’un déploiement vers Adobe I/O Runtime pour une utilisation par AEM services d’auteur as a Cloud Service, seules deux variables d’environnement sont requises :

+ `AIO_runtime_namespace` pointe vers App Builder Workspace à déployer.
+ `AIO_runtime_auth` sont les informations d’identification d’authentification de l’espace de travail App Builder ;

Les autres variables standard définies dans la variable `.env` sont implicitement fournis par AEM as a Cloud Service lorsqu’il appelle le programme de travail des Assets compute.

## Espace de travail de développement

Parce que ce projet a été généré à l’aide de `aio app init` en utilisant la variable `Development` espace de travail, `AIO_runtime_namespace` est automatiquement défini sur `81368-wkndaemassetcompute-development` avec la correspondance `AIO_runtime_auth` dans notre local `.env` fichier .  Si `.env` existe dans le répertoire utilisé pour émettre la commande deploy, ses valeurs sont utilisées, sauf si elles sont remplacées par le biais d’une exportation de variable au niveau du système d’exploitation, ce qui est la méthode suivante : [évaluation et production](#stage-and-production) les espaces de travail sont ciblés.

![déploiement de l’application aio à l’aide de variables .env](./assets/runtime/development__aio.png)

Pour déployer sur l’espace de travail défini dans les projets `.env` fichier :

1. Ouvrez la ligne de commande à la racine du projet Asset compute.
1. Exécutez la commande `aio app deploy`
1. Exécutez la commande `aio app get-url` pour obtenir l’URL du programme de travail à utiliser dans AEM profil de traitement as a Cloud Service afin de référencer ce programme de travail d’Asset compute personnalisé. Si le projet contient plusieurs programmes de travail, des URL distinctes pour chaque programme de travail sont répertoriées.

Si les environnements de développement local et de développement as a Cloud Service AEM utilisent des déploiements d’Asset compute distincts, les déploiements vers AEM développement as a Cloud Service peuvent être gérés de la même manière que [Déploiements dans les environnements intermédiaires et de production](#stage-and-production).

## Espaces de travail intermédiaires et de production{#stage-and-production}

Le déploiement sur les espaces de travail intermédiaires et de production est généralement effectué par votre système CI/CD de votre choix. Le projet Asset compute doit être déployé discrètement dans chaque espace de travail (intermédiaire puis production).

La définition de variables d’environnement vraies remplace les valeurs des variables du même nom dans `.env`.

![déploiement de l’application aio à l’aide de variables d’exportation](./assets/runtime/stage__export-and-aio.png)

L’approche générale, généralement automatisée par un système CI/CD, pour le déploiement dans les environnements d’évaluation et de production est la suivante :

1. Assurez-vous que la variable [Module npm d’Adobe I/O CLI et module externe d’Asset compute](../set-up/development-environment.md#aio) sont installés
1. Extraire le projet Asset compute à déployer à partir de Git
1. Définissez les variables d’environnement avec les valeurs qui correspondent à l’espace de travail cible (d’évaluation ou de production).
   + Les deux variables requises sont les suivantes : `AIO_runtime_namespace` et `AIO_runtime_auth` et sont obtenus par espace de travail dans Adobe I/O Developer Console via le __Tout télécharger__ fonction .

![Adobe Developer Console : espace de noms AIO Runtime et Auth](./assets/runtime/stage-auth-namespace.png)

Les valeurs de ces clés peuvent être définies à partir de la ligne de commande :

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si les objets Worker Asset compute requièrent d’autres variables, telles que l’espace de stockage dans le cloud, celles-ci doivent également être exportées en tant que variables d’environnement.

1. Une fois que toutes les variables d’environnement sont définies pour que l’espace de travail cible soit déployé, exécutez la commande deploy :
   + `aio app deploy`
1. Les URL de traitement référencées par le profil de traitement as a Cloud Service AEM sont également disponibles via :
   + `aio app get-url`.

Si la version du projet d’Asset compute change, les URL du programme de travail changent également pour refléter la nouvelle version, et l’URL doit être mise à jour dans les profils de traitement.

## Approvisionnement de l’API Workspace{#workspace-api-provisioning}

When [Configuration du projet App Builder dans Adobe I/O](../set-up/app-builder.md) pour prendre en charge le développement local, un nouvel espace de travail de développement a été créé et __asset compute, événements E/S__ et __API de gestion des événements I/O__ ont été ajoutées à .

Le __asset compute, événements E/S__ et __API de gestion des événements I/O__ Les API ne sont explicitement ajoutées qu’aux espaces de travail utilisés pour le développement local. Les espaces de travail qui s’intègrent (exclusivement) aux environnements as a Cloud Service AEM font __not__ Ces API doivent être explicitement ajoutées, car les API sont mises naturellement à la disposition d’AEM as a Cloud Service.
