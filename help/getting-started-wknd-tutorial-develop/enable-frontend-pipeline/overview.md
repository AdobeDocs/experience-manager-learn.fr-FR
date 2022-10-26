---
title: Activation du pipeline frontal pour l’archétype de projet d’AEM standard
description: Découvrez comment activer un pipeline front-end pour un projet d’AEM standard pour un déploiement plus rapide des ressources statiques telles que CSS, JavaScript, Polices et Icônes. Séparation également du développement front-end du développement principal de la pile complète sur AEM.
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# Activation du pipeline frontal pour l’archétype de projet d’AEM standard{#enable-front-end-pipeline-standard-aem-project}

Découvrez comment activer le [AEM projet WKND Sites](https://github.com/adobe/aem-guides-wknd) (ou Projet d’AEM standard) créé à l’aide de [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype) pour déployer des ressources front-end telles que CSS, JavaScript, Polices et Icônes à l’aide d’un pipeline front-end afin d’accélérer le cycle de développement vers déploiement. Séparation du développement front-end et du développement principal en pile sur AEM. Vous découvrez également comment ces ressources front-end sont __not__ diffusé à partir du référentiel d’AEM, mais à partir du réseau de diffusion de contenu, un changement de paradigme de diffusion.


Un nouveau pipeline front-end est créé dans Adobe Cloud Manager qui ne crée et ne déploie que les éléments `ui.frontend` artefacts sur le réseau de diffusion de contenu intégré et informations AEM sur son emplacement. En AEM pendant la génération de HTML de la page web, la variable `<link>` et `<script>` balises, reportez-vous à cet emplacement d’artefact dans `href` valeur d’attribut.

Cependant, après la conversion de projet AEM WKND Sites, les développeurs front-end peuvent travailler séparément et parallèlement à tout développement principal de pile sur AEM, qui possède ses propres pipelines de déploiement.

>[!IMPORTANT]
>
>En règle générale, le pipeline front-end est généralement utilisé avec la variable [Création AEM site rapide](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), il existe un tutoriel connexe [Prise en main d’AEM Sites - Création rapide d’un site](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) pour en savoir plus. Dans ce tutoriel et les vidéos qui lui sont associées, vous découvrez les références qui y sont faites, ceci afin de vous assurer que des différences subtiles sont mises en évidence et qu&#39;il existe une comparaison directe ou indirecte pour expliquer les concepts cruciaux.


Un [tutoriel en plusieurs étapes](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) décrit la mise en oeuvre d’un site AEM pour une marque de style de vie fictive WKND à l’aide de la fonction Création rapide de site . Vérification de la [Workflow de thème](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) il est également utile de comprendre les rouages front-end du pipeline.

## Présentation, avantages et considérations du pipeline frontal

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>Cela s’applique uniquement aux AEM as a Cloud Service et non aux déploiements Adobe Cloud Manager basés sur AMS.

## Prérequis

L’étape de déploiement de ce tutoriel se déroule dans Adobe Cloud Manager, assurez-vous que vous disposez d’un __Responsable de déploiement__ rôle, voir Gestion de cloud [Définitions de rôle](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

Veillez à utiliser la variable [Programme Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) et [Environnement de développement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) lorsque vous suivez ce tutoriel.

## Étapes suivantes {#next-steps}

Un tutoriel détaillé décrit le [AEM projet WKND Sites](https://github.com/adobe/aem-guides-wknd) conversion pour l’activer pour le pipeline front-end.

Qu&#39;attends-tu ? Démarrez le tutoriel en accédant à la [Examinez le projet de pile complète](review-uifrontend-module.md) chapitre et récapituler le cycle de vie du développement front-end dans le contexte du projet AEM Sites standard.

