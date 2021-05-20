---
title: Déployer des objets Worker Asset compute dans Adobe I/O Runtime pour une utilisation avec AEM en tant que Cloud Service
description: 'Les projets d’Asset compute, ainsi que les travailleurs qu’ils contiennent, doivent être déployés dans Adobe I/O Runtime pour être utilisés par AEM en tant que Cloud Service. '
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Intégrations, développement
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# Déploiement sur Adobe I/O Runtime

Les projets d’Asset compute, ainsi que les travailleurs qu’ils contiennent, doivent être déployés vers Adobe I/O Runtime via l’interface de ligne de commande de l’Adobe I/O pour être utilisés par AEM en tant que Cloud Service.

Lors du déploiement vers Adobe I/O Runtime pour une utilisation par AEM as a Cloud Service Author Services, seules deux variables d’environnement sont requises :

+ `AIO_runtime_namespace` pointe vers l’Adobe Project Firefly Workspace à déployer.
+ `AIO_runtime_auth` sont les informations d’identification de l’espace de travail Adobe Project Firefly

Les autres variables standard définies dans le fichier `.env` sont implicitement fournies par AEM en tant que Cloud Service lors de l’appel du programme de travail des Assets compute.

## Espace de travail de développement

Comme ce projet a été généré à l’aide de `aio app init` à l’aide de l’espace de travail `Development`, `AIO_runtime_namespace` est automatiquement défini sur `81368-wkndaemassetcompute-development` avec la balise `AIO_runtime_auth` correspondante dans notre fichier `.env` local.  Si un fichier `.env` existe dans le répertoire utilisé pour émettre la commande de déploiement, ses valeurs sont utilisées, à moins qu’elles ne soient remplacées par une exportation de variable au niveau du système d’exploitation, ce qui est la manière dont les espaces de travail [stage et production](#stage-and-production) sont ciblés.

![déploiement de l’application aio à l’aide de variables .env](./assets/runtime/development__aio.png)

Pour déployer vers l’espace de travail défini dans le fichier projets `.env` :

1. Ouvrez la ligne de commande à la racine du projet Asset compute.
1. Exécutez la commande `aio app deploy`
1. Exécutez la commande `aio app get-url` pour obtenir l’URL du programme de travail à utiliser dans AEM as a Cloud Service Processing Profile afin de référencer ce programme de travail d’Asset compute personnalisé. Si le projet contient plusieurs programmes de travail, des URL distinctes pour chaque programme de travail sont répertoriées.

Si les environnements de développement local et d’AEM en tant que Cloud Service utilisent des déploiements d’Asset compute distincts, les déploiements vers AEM en tant que développement Cloud Service peuvent être gérés de la même manière que les déploiements d’évaluation et de production ](#stage-and-production).[

## Espaces de travail intermédiaires et de production{#stage-and-production}

Le déploiement sur les espaces de travail intermédiaires et de production est généralement effectué par votre système CI/CD de votre choix. Le projet Asset compute doit être déployé discrètement dans chaque espace de travail (intermédiaire puis production).

La définition de variables d’environnement vraies remplace les valeurs des variables du même nom dans `.env`.

![déploiement de l’application aio à l’aide de variables d’exportation](./assets/runtime/stage__export-and-aio.png)

L’approche générale, généralement automatisée par un système CI/CD, pour le déploiement dans les environnements d’évaluation et de production est la suivante :

1. Assurez-vous que le module [Adobe I/O CLI npm et le module externe Asset compute](../set-up/development-environment.md#aio) sont installés.
1. Extraire le projet Asset compute à déployer à partir de Git
1. Définissez les variables d’environnement avec les valeurs qui correspondent à l’espace de travail cible (d’évaluation ou de production).
   + Les deux variables requises sont `AIO_runtime_namespace` et `AIO_runtime_auth` et sont obtenues par espace de travail dans Adobe I/O Developer Console via la fonction __Télécharger tout__ de Workspace.

![Adobe Developer Console : espace de noms AIO Runtime et Auth](./assets/runtime/stage-auth-namespace.png)

Les valeurs de ces clés peuvent être définies à partir de la ligne de commande :

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si les objets Worker Asset compute requièrent d’autres variables, telles que l’espace de stockage dans le cloud, celles-ci doivent également être exportées en tant que variables d’environnement.

1. Une fois que toutes les variables d’environnement sont définies pour que l’espace de travail cible soit déployé, exécutez la commande deploy :
   + `aio app deploy`
1. Les URL de traitement référencées par l’AEM en tant que profil de traitement de Cloud Service sont également disponibles via :
   + `aio app get-url`.

Si la version du projet d’Asset compute change, les URL du programme de travail changent également pour refléter la nouvelle version, et l’URL doit être mise à jour dans les profils de traitement.

## Approvisionnement de l’API Workspace{#workspace-api-provisioning}

Lorsque [la configuration du projet Adobe Project Firefly dans Adobe I/O](../set-up/firefly.md) pour prendre en charge le développement local, un nouvel espace de travail de développement a été créé et __Asset compute, événements I/O__ et __API de gestion des événements I/O__ ont été ajoutés.

Les API __Asset compute, I/O Events__ et __I/O Events Management API__ ne sont explicitement ajoutées qu’aux espaces de travail utilisés pour le développement local. Les espaces de travail qui s’intègrent (exclusivement) à AEM en tant qu’environnements de Cloud Service __et non__ ont besoin que ces API soient explicitement ajoutées, car les API sont mises naturellement à la disposition d’AEM en tant que Cloud Service.
