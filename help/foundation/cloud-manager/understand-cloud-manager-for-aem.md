---
title: Présentation d’Adobe Cloud Manager
description: Adobe Cloud Manager offre une solution simple et robuste qui permet une gestion, des introspections et un libre-service aisés des environnements AEM.
sub-product: cloud-manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 16%

---

# Présentation d’Adobe Cloud Manager

Adobe Cloud Manager offre une solution simple et robuste qui permet une gestion, des introspections et un libre-service aisés des environnements AEM.

## Présentation de Cloud Manager

Cette série vidéo explore les fonctionnalités clés de Cloud Manager pour AEM, notamment :

* [Programmes](#programs)
* [Environnements](#environments)
* [Rapports](#reports)
* [Pipeline de production CI/CD](#cicd-production-pipeline)
* [Pipelines hors production CI/CD](#cicd-non-production-pipeline)
* [Activité](#activity)

Pour une présentation complète, consultez le [Guide de l’utilisateur de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## Programmes {#programs}

[Programmes Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) représentent des ensembles d’environnements AEM prenant en charge des ensembles logiques d’initiatives commerciales, correspondant généralement à un contrat de niveau de service (SLA) acheté. Par exemple, un programme peut représenter les ressources AEM pour prendre en charge les sites Web publics globaux, tandis qu’un autre programme représente un DAM central interne.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Environnements {#environments}

[Environnements Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) sont composés d’instances AEM Author, AEM Publish et Dispatcher. Les différents environnements prennent en charge les rôles et peuvent être engagés à l’aide de différents pipelines CI/CD (décrits ci-dessous). Les environnements Cloud Manager ont généralement un environnement de production et un environnement d’évaluation.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Rapports {#reports}

[Rapports Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) fournir une vue des environnements du programme et des instances d’AEM par le biais d’un ensemble de graphiques qui génèrent des rapports et effectuent le suivi de diverses mesures pour chaque instance AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Pipeline de production CI/CD {#cicd-production-pipeline}

*[Utilisation du pipeline CI/CD dans Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Les séries de vidéos plongent en profondeur dans l’exécution du pipeline de production, notamment l’exploration des déploiements échoués et réussis.*

>[!NOTE]
>
> Dans ces vidéos, les délais de création, de test et de déploiement ont été accélérés afin de réduire la durée de la vidéo. L’exécution complète d’un pipeline prend généralement 45 minutes ou plus (y compris les tests de performances obligatoires de 30 minutes), selon la taille du projet, le nombre d’instances AEM et les processus UAT.

### Configuration

Le [Pipeline de production CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) configuration définit le déclencheur qui lance le pipeline, ainsi que les paramètres contrôlant le déploiement en production et les paramètres de test de performances.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Exécution du pipeline

Le [Pipeline de production CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) est utilisé pour créer et déployer du code par le biais de l’environnement d’évaluation vers l’environnement de production, ce qui réduit le temps d’évaluation.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Pipelines hors production CI/CD {#cicd-non-production-pipeline}

[Les pipelines CI/CD hors production sont divisés en deux catégories : les pipelines de qualité du code et les pipelines de déploiement. ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) Les pipelines de qualité du code canalisent tout le code d’une branche Git pour génération et évaluation par rapport à l’analyse de la qualité du code de Cloud Manager. Les pipelines de déploiement prennent en charge le déploiement automatisé de code du référentiel Git vers tout environnement hors production, c’est-à-dire tout environnement d’AEM configuré qui n’est ni dans l’environnement intermédiaire ni dans l’environnement de production.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Activité {#activity}

Cloud Manager fournit une vue consolidée de l’activité d’un programme, répertoriant toutes les exécutions de pipeline CI/CD, à la fois en production et hors production, ce qui permet de connaître l’activité passée et présente, et les détails de toute activité peuvent être examinés.

Cloud Manager s’intègre également à un niveau par utilisateur avec [Notifications Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), offrant une vue d’ensemble des événements et des actions qui les intéressent.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
