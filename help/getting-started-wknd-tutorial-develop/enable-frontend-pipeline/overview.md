---
title: Activer le pipeline front-end pour l’archétype de projet standard d’AEM
description: Découvrez comment activer un pipeline front-end pour un projet standard AEM en vue d’un déploiement plus rapide des ressources statiques telles que CSS, JavaScript, des polices et des icônes. Le développement front-end est séparé du développement back-end full-stack sur AEM.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 100%

---


# Activer le pipeline front-end pour l’archétype de projet standard d’AEM{#enable-front-end-pipeline-standard-aem-project}

Découvrez comment activer le [projet AEM Sites WKND](https://github.com/adobe/aem-guides-wknd) (ou projet standard d’AEM) créé à l’aide de l’[archétype de projet d’AEM](https://github.com/adobe/aem-project-archetype) pour déployer des ressources front-end telles que CSS, JavaScript, des polices et des icônes à l’aide d’un pipeline front-end qui accélère le cycle du développement vers le déploiement. Le développement front-end est séparé du développement back-end full-stack sur AEM. Vous découvrirez également que ces ressources front-end ne sont __pas__ diffusées à partir du référentiel AEM, mais à partir du réseau CDN, un changement de paradigme de diffusion.


Un nouveau pipeline front-end est créé dans Adobe Cloud Manager, qui ne crée et ne déploie que les artefacts `ui.frontend` sur le réseau CDN intégré et informe AEM sur son emplacement. Dans AEM, pendant la génération HTML de la page web, les balises `<link>` et `<script>` font référence à cet emplacement d’artefact dans la valeur d’attribut `href`.

Cependant, après la conversion du projet AEM Sites WKND, les développeurs et développeuses front-end peuvent travailler séparément et parallèlement à tout développement back-end full-stack sur AEM, qui possède ses propres pipelines de déploiement.

>[!IMPORTANT]
>
>En règle générale, le pipeline front-end est utilisé avec la [création rapide de site d’AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=fr). Pour en savoir plus, consultez le tutoriel connexe [Prise en main d’AEM Sites - création rapide de site](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=fr). Vous rencontrerez dans ce tutoriel et les vidéos qui lui sont associées des références à ceci, afin de garantir que les différences subtiles sont expliquées et que les comparaisons directes ou indirectes expliquent les concepts indispensables.


Un [tutoriel en plusieurs étapes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=fr) décrit l’mplémentation d’un site AEM pour une marque de style de vie fictive, WKND, à l’aide de la fonction Création rapide de site. Nous vous invitons également à vérifier le [Workflow de thème](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=fr) pour comprendre les rouages du pipeline front-end.

## Présentation, avantages et considérations du pipeline front-end

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>Cette section s’applique uniquement à AEM as a Cloud Service et non aux déploiements d’Adobe Cloud Manager basés sur AMS.

## Prérequis

L’étape de déploiement de ce tutoriel se déroule dans Adobe Cloud Manager. Assurez-vous que vous disposez d’un rôle de __Responsable de déploiement__ et référez-vous aux [Définitions des rôles](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=fr#role-definitions) de Cloud Manager.

Veillez à utiliser le [programme Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=fr) et l‘[environnement de développement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=fr) lorsque vous suivez ce tutoriel.

## Étapes suivantes {#next-steps}

Un tutoriel détaillé décrit la conversion du [projet AEM Sites WKND](https://github.com/adobe/aem-guides-wknd) pour une activation sur le pipeline front-end.

Qu’attendez-vous ? Commencez le tutoriel par le chapitre [Examiner le projet full-stack](review-uifrontend-module.md) et reprenez le cycle de vie du développement front-end dans le contexte du projet standard d’AEM Sites.

