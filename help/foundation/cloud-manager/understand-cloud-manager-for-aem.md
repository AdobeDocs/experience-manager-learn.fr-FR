---
title: Comprendre Adobe Cloud Manager
description: Adobe Cloud Manager offre une solution simple, mais robuste, qui permet une gestion, un contrôle et un libre-service faciles des environnements AEM.
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '512'
ht-degree: 100%

---

# Comprendre Adobe Cloud Manager

Adobe Cloud Manager offre une solution simple, mais robuste, qui permet une gestion, un contrôle et un libre-service faciles des environnements AEM.

## Vue d’ensemble de Cloud Manager

Cette série de vidéos explore les fonctionnalités clés de Cloud Manager pour AEM, notamment :

* [Programmes](#programs)
* [Environnements](#environments)
* [Rapports](#reports)
* [Pipeline de production CI/CD](#cicd-production-pipeline)
* [Pipelines hors production CI/CD](#cicd-non-production-pipeline)
* [Activity](#activity)

Pour une présentation complète, consultez le [Guide d’utilisation de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=fr).

## Programmes {#programs}

[Les programmes Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=fr) représentent des ensembles d’environnements AEM prenant en charge des ensembles logiques d’initiatives commerciales, correspondant généralement à un contrat de niveau de service (SLA) acheté. Par exemple, un programme peut représenter les ressources AEM pour prendre en charge les sites web publics globaux, tandis qu’un autre programme représente une gestion des ressources numériques centrale interne.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## Environnements {#environments}

[Les environnements Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=fr) sont composés d’instances de création AEM, de publication AEM et du Dispatcher. Différents environnements prennent en charge les rôles et peuvent être utilisés à l’aide de différents pipelines CI/CD (décrits ci-dessous). Les environnements Cloud Manager ont généralement un environnement de production et un environnement d’évaluation.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## Rapports {#reports}

[Les rapports Cloud Manager ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=fr) fournissent une vue des environnements du programme et des instances AEM au moyen d’un ensemble de graphiques qui génèrent des rapports et effectuent le suivi de diverses mesures pour chaque instance AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## Pipeline de production CI/CD {#cicd-production-pipeline}

*[La série de vidéos Utilisation du pipeline CI/CD dans Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) plonge en profondeur dans l’exécution du pipeline de production, notamment l’exploration des déploiements échoués et réussis.*

>[!NOTE]
>
> Dans ces vidéos, la durée de création, de test et de déploiement a été accélérée afin de réduire la durée de la vidéo. L’exécution complète d’un pipeline prend généralement 45 minutes ou plus (y compris les tests de performances obligatoires de 30 minutes), selon la taille du projet, le nombre d’instances AEM et les processus UAT.

### Configuration

La configuration du [pipeline de production CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=fr) définit le déclencheur qui lancera le pipeline, les paramètres contrôlant le déploiement en production et les paramètres de test de performances.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### Exécution du pipeline

Le [pipeline de production CI/CD](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=fr) est utilisé pour créer et déployer du code par le biais de l’environnement d’évaluation vers l’environnement de production, ce qui réduit le temps d’évaluation.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## Pipelines hors production CI/CD {#cicd-non-production-pipeline}

[Les pipelines CI/CD hors production](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=fr) sont divisés en deux catégories : les pipelines de qualité du code et les pipelines de déploiement. Les pipelines de qualité du code canalisent tout le code d’une branche Git pour génération et évaluation par rapport à l’analyse de la qualité du code de Cloud Manager. Les pipelines de déploiement prennent en charge le déploiement automatisé du code du référentiel Git vers tout environnement hors production, c&#39;est-à-dire tout environnement AEM provisionné qui n&#39;est ni en évaluation ni en production.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## Activity {#activity}

Cloud Manager fournit une vue consolidée de l’activité d’un programme, répertoriant toutes les exécutions de pipeline CI/CD, tant en production que hors production, ce qui permet de connaître l’activité passée et présente, et permet aux détails de toute activité d’être examinés.

Cloud Manager s’intègre également à un niveau selon l’utilisateur ou l’utilisatrice avec les [Notifications Adobe Experience Cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=fr), offrant une vue d’ensemble des événements et des actions importants.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
