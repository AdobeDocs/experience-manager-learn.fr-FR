---
title: Comprendre Adobe Cloud Manager
description: Adobe Cloud Manager offre une solution simple mais robuste qui permet une gestion, une introspection et un libre-service aisés des environnements AEM.
sub-product: cloud manager, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architecte
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 22%

---


# Comprendre Adobe Cloud Manager

Adobe Cloud Manager offre une solution simple mais robuste qui permet une gestion, une introspection et un libre-service aisés des environnements AEM.

## Présentation de Cloud Manager

Cette série de vidéos explore les principales fonctionnalités de Cloud Manager pour AEM :

* [Programmes](#programs)
* [Environnements](#environments)
* [Rapports](#reports)
* [Pipeline de production CI/CD](#cicd-production-pipeline)
* [Pipelines non productifs CI/CD](#cicd-non-production-pipeline)
* [Activité](#activity)

Pour une présentation complète, consultez le [Guide de l’utilisateur de Cloud Manager](https://docs.adobe.com/content/help/fr-FR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## Programmes {#programs}

[Les ](https://docs.adobe.com/content/help/fr-FR/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) programmes Cloud Manager représentent des ensembles d’environnements AEM prenant en charge des ensembles logiques d’initiatives commerciales, généralement correspondant à un contrat de niveau de service (SLA) acheté. Par exemple, un Programme peut représenter les ressources AEM pour soutenir les sites Web publics mondiaux, tandis qu&#39;un autre Programme représente un DAM central interne.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## Environnements {#environments}

[Les ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) environnements Cloud Manager sont composés d’instances d’auteur AEM, de publication AEM et de répartiteur. Différents environnements prennent en charge des rôles et peuvent être engagés à l&#39;aide de différents pipelines CI/CD (décrits ci-dessous). Les environnements de Cloud Manager ont généralement un environnement de production et un environnement d’étape.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## Rapports {#reports}

[Les rapports Cloud Manager fournissent une vue des environnements du programme et des instances AEM au moyen d’un ensemble de graphiques qui génèrent des rapports et effectuent le suivi de diverses mesures pour chaque instance AEM.](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html)

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## Pipeline de production CI/CD {#cicd-production-pipeline}

*[La série vidéo Utilisation du pipeline CI/CD dans Adobe Cloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Manager permet de plonger en profondeur dans l’exécution du pipeline de production, y compris l’exploration de déploiements réussis ou non.*

>[!NOTE]
>
> Au cours de ces vidéos, les délais de création, de test et de déploiement ont été accélérés afin de réduire la durée de la vidéo. Une exécution complète d’un pipeline prend généralement 45 minutes ou plus (y compris les tests de performances obligatoires de 30 minutes), selon la taille du projet, le nombre d’instances d’AEM et les processus UAT.

### Configuration

La configuration du [pipeline de production CI/CD](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) définit le déclencheur qui lancera le pipeline, les paramètres contrôlant le déploiement de production et les paramètres de test de performances.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### Exécution du pipeline

Le [pipeline de production CI/CD](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) est utilisé pour créer et déployer du code via Stage vers l&#39;environnement de production, ce qui réduit le temps de valorisation.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## Pipelines de non-production CI/CD {#cicd-non-production-pipeline}

[Les pipelines CI/CD hors production sont divisés en deux catégories : les pipelines de qualité du code et les pipelines de déploiement. ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) Les pipelines de qualité du code canalisent tout le code d’une branche Git pour génération et évaluation par rapport à l’analyse de la qualité du code de Cloud Manager. Les pipelines de déploiement prennent en charge le déploiement automatisé du code depuis le référentiel Git vers tout environnement hors production, c&#39;est-à-dire tout environnement AEM mis en service qui n&#39;est ni Stage ni Production.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## Activité {#activity}

Cloud Manager fournit une vue consolidée dans une activité de Programme, qui répertorie toutes les exécutions de pipeline CI/CD, tant en production qu’en non-production, ce qui permet une visibilité sur l’activité passée et présente, et les détails de toute activité peuvent être examinés.

Cloud Manager s’intègre également à un niveau utilisateur par utilisateur avec [Adobe Experience Cloud Notifications](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html), fournissant ainsi une vue omniprésente dans les événements et actions présentant un intérêt.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
