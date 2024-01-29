---
title: Déployer des programmes de travail d’Asset Compute sur Adobe I/O Runtime pour une utilisation avec AEM as a Cloud Service
description: Les projets Asset Compute et les programmes de travail inclus doivent être déployés sur Adobe I/O Runtime pour être utilisés avec AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 165
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '645'
ht-degree: 100%

---

# Déployer sur Adobe I/O Runtime

Les projets Asset Compute et les programmes de travail inclus doivent être déployés sur Adobe I/O Runtime via Adobe I/O CLI pour être utilisés avec AEM as a Cloud Service.

Lors du déploiement vers Adobe I/O Runtime pour une utilisation par les services de création AEM as a Cloud Service, seules deux variables d’environnement sont requises :

+ `AIO_runtime_namespace` indique l’espace de travail App Builder dans lequel le déploiement doit s’effectuer
+ `AIO_runtime_auth` sont les informations d’authentification de l’espace de travail App Builder

Les autres variables standard définies dans le fichier `.env` sont implicitement fournies par AEM as a Cloud Service lorsqu’il appelle le programme de travail d’Asset Compute.

## Espace de travail de développement

Comme ce projet a été généré à l’aide de `aio app init` dans l’espace de travail `Development`, `AIO_runtime_namespace` est automatiquement défini sur `81368-wkndaemassetcompute-development` avec la valeur correspondante `AIO_runtime_auth` dans notre fichier local `.env`.  Si le fichier `.env` existe dans le répertoire utilisé pour lancer la commande de déploiement, ses valeurs sont utilisées, sauf si elles sont remplacées par un export de variable au niveau du système d’exploitation. Il s’agit de la façon dont les espaces de travail d’[évaluation et de production](#stage-and-production) sont ciblés.

![déploiement de l’application aio à l’aide de variables .env](./assets/runtime/development__aio.png).

Pour déployer dans l’espace de travail défini dans le fichier `.env` du projet :

1. Ouvrez la ligne de commande à la racine du projet Asset Compute.
1. Exécutez la commande `aio app deploy`.
1. Exécutez la commande `aio app get-url` pour obtenir l’URL du programme de travail à utiliser dans le profil de traitement AEM as a Cloud Service afin de référencer ce programme de travail Asset Compute personnalisé. Si le projet contient plusieurs programmes de travail, des URL distinctes pour chaque programme de travail sont répertoriées.

Si les environnements de développement local et de développement AEM as a Cloud Service utilisent des déploiements Asset Compute distincts, les déploiements vers l’environnement de développement AEM as a Cloud Service peuvent être gérés de la même manière que les [déploiements vers les environnements d’évaluation et de production](#stage-and-production).

## Espaces de travail d’évaluation et de production{#stage-and-production}

Le déploiement vers les espaces de travail d’évaluation et de production peut être effectué par le système CI/CD de votre choix. Le projet Asset Compute doit être déployé individuellement sur chaque espace de travail (évaluation, puis production).

La définition de variables d’environnement « true » remplace les valeurs des variables du même nom dans `.env`.

![déploiement de l’application aio à l’aide de variables d’export](./assets/runtime/stage__export-and-aio.png).

L’approche typique, souvent automatisée par un système CI/CD, pour le déploiement dans les environnements d’évaluation et de production est la suivante :

1. Assurez-vous que le [module npm d’Adobe I/O CLI et le plug-in Asset Compute](../set-up/development-environment.md#aio) sont installés.
1. Consultez le projet Asset Compute à déployer à partir de Git.
1. Définissez les variables d’environnement avec les valeurs qui correspondent à l’espace de travail cible (évaluation ou production).
   + Les deux variables requises sont `AIO_runtime_namespace` et `AIO_runtime_auth`. Elles sont obtenues, pour chaque espace de travail, dans Adobe I/O Developer Console via la fonction __Télécharger tout__ de l’espace de travail.

![Espace de noms et informations d’authentification d’Adobe Developer Console - AIO Runtime](./assets/runtime/stage-auth-namespace.png)

Les valeurs de ces clés peuvent être définies à l’aide de commandes d’export sur la ligne de commande :

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Si les programmes de travail d’Asset Compute nécessitent d’autres variables, telles que l’espace de stockage, celles-ci doivent également être exportées en tant que variables d’environnement.

1. Une fois que toutes les variables d’environnement sont définies pour le déploiement de l’espace de travail cible, exécutez la commande de déploiement :
   + `aio app deploy`
1. Le ou les URL des programmes de travail référencées par le profil de traitement AEM as a Cloud Service sont également disponibles via :
   + `aio app get-url`.

Si la version du projet Asset Compute est modifiée, le ou les URL des programmes de travail lui emboîtent le pas pour refléter la nouvelle version. L’URL doit également être mise à jour dans les profils de traitement.

## Approvisionnement de l’API Workspace{#workspace-api-provisioning}

Lors de la [configuration du projet App Builder dans Adobe I/O](../set-up/app-builder.md) pour permettre le développement local, un nouvel espace de travail de développement a été créé, comprenant __Asset Compute, les événements I/O__ et les __API /O Events Management__.

Les API __Asset Compute, les événements I/O__ et les __API I/O Events Management__ ne sont explicitement ajoutées qu’aux espaces de travail utilisés pour le développement local. Les espaces de travail réservés (exclusivement) aux environnements AEM as a Cloud Service n’ont __pas__ besoin que ces API soient explicitement ajoutées, car elles sont disponibles par défaut pour AEM as a Cloud Service.
