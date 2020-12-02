---
title: Déployer des employés d'Asset compute dans Adobe I/O Runtime pour une utilisation avec AEM en tant que Cloud Service
description: 'Les projets d''Asset compute, et les travailleurs qu''ils contiennent, doivent être déployés à Adobe I/O Runtime pour être utilisés par AEM comme Cloud Service. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 0%

---


# Déploiement sur Adobe I/O Runtime

Les projets d&#39;Asset compute, et les travailleurs qu&#39;ils contiennent, doivent être déployés à Adobe I/O Runtime par l&#39;intermédiaire de l&#39;interface de ligne de commande Adobe I/O pour être utilisés par AEM en tant que Cloud Service.

Lors d’un déploiement vers Adobe I/O Runtime pour une utilisation par AEM en tant que services d’auteur Cloud Service, seules deux variables d’environnement sont requises :

+ `AIO_runtime_namespace` pointe l&#39;Adobe Project Firefly Workspace à déployer
+ `AIO_runtime_auth` sont les informations d&#39;identification d&#39;authentification de l&#39;espace de travail Project Firefly Adobe

Les autres variables standard définies dans le fichier `.env` sont implicitement fournies par AEM en tant que Cloud Service lorsqu’il appelle l’agent d’Asset compute.

## Espace de travail de développement

Comme ce projet a été généré à l&#39;aide de `aio app init` l&#39;espace de travail `Development`, `AIO_runtime_namespace` est automatiquement défini sur `81368-wkndaemassetcompute-development` avec la valeur `AIO_runtime_auth` correspondante dans notre fichier `.env` local.  Si un fichier `.env` existe dans le répertoire utilisé pour émettre la commande deploy, ses valeurs sont utilisées, à moins qu&#39;elles ne soient remplacées par une exportation de variables au niveau du système d&#39;exploitation, ce qui correspond à la manière dont les espaces de travail [stage et production](#stage-and-production) sont ciblés.

![déploiement de l’application aio à l’aide de variables .env](./assets/runtime/development__aio.png)

Pour effectuer un déploiement dans l&#39;espace de travail défini dans le fichier projets `.env` :

1. Ouvrez la ligne de commande à la racine du projet d&#39;Asset compute.
1. Exécutez la commande `aio app deploy`
1. Exécutez la commande `aio app get-url` pour obtenir l’URL du collaborateur à utiliser dans l’AEM en tant que Profil de traitement du Cloud Service pour référencer ce collaborateur d’Assets compute personnalisé. Si le projet contient plusieurs programmes de travail, des URL distinctes pour chaque programme de travail sont répertoriées.

Si le développement local et l&#39;AEM en tant qu&#39;environnements de développement Cloud Service utilisent des déploiements d&#39;Asset compute distincts, les déploiements vers AEM en tant que développement Cloud Service peuvent être gérés de la même manière que les déploiements [Phase et Production](#stage-and-production).

## Espaces de travail Phase et Production{#stage-and-production}

Le déploiement sur les espaces de travail Stage et Production est généralement effectué par votre système de CI/CD de choix. Le projet d’Asset compute doit être déployé discrètement dans chaque espace de travail (phase, puis production).

La définition de variables d&#39;environnement vraies remplace les valeurs des variables portant le même nom dans `.env`.

![déploiement de l’application aio à l’aide de variables d’exportation](./assets/runtime/stage__export-and-aio.png)

L&#39;approche générale, généralement automatisée par un système de CI/CD, pour le déploiement sur les environnements d&#39;étape et de production est la suivante :

1. Assurez-vous que le module [Adobe I/O CLI npm et le module externe d’Asset compute ](../set-up/development-environment.md#aio) sont installés.
1. Consulter le projet d&#39;Asset compute à déployer à partir de Git
1. Définissez les variables d&#39;environnement avec les valeurs correspondant à l&#39;espace de travail de cible (Phase ou Production).
   + Les deux variables requises sont `AIO_runtime_namespace` et `AIO_runtime_auth` et sont obtenues par espace de travail dans Adobe I/O Developer Console via la fonction __Télécharger tout__ de Workspace.

![Adobe Developer Console - Espace de nommage d&#39;exécution AIO et authentification](./assets/runtime/stage-auth-namespace.png)

Les valeurs de ces clés peuvent être définies en exécutant des commandes d’exportation à partir de la ligne de commande :

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si vos collaborateurs d’Asset compute ont besoin d’autres variables, par exemple à l’enregistrement du cloud, elles doivent également être exportées en tant que variables d’environnement.

1. Une fois que toutes les variables d&#39;environnement sont définies pour l&#39;espace de travail de cible sur lequel le déploiement doit s&#39;effectuer, exécutez la commande deploy :
   + `aio app deploy`
1. Les URL de travail référencées par l’AEM en tant que Profil de traitement Cloud Service sont également disponibles via :
   + `aio app get-url`.

Si la version du projet d’Asset compute change, la ou les URL de travail changent également pour refléter la nouvelle version et l’URL doit être mise à jour dans les Profils de traitement.

## Approvisionnement de l’API Workspace{#workspace-api-provisioning}

Lorsque [la configuration du projet Adobe Project Firefly à Adobe I/O](../set-up/firefly.md) pour prendre en charge le développement local, un nouvel espace de travail de développement a été créé et __Asset compute, Événements d&#39;E/S__ et __API de gestion des Événements d&#39;E/S__ y ont été ajoutés.

Les API __Asset compute, Événements d&#39;E/S__ et __API de gestion des Événements d&#39;E/S__ ne sont explicitement ajoutées qu&#39;aux espaces de travail utilisés pour le développement local. Les espaces de travail qui s’intègrent (exclusivement) à l’AEM en tant qu’environnements Cloud Service n’ont __pas__ besoin de ces API explicitement ajoutées, car les API sont rendues disponibles naturellement pour AEM en tant que Cloud Service.
