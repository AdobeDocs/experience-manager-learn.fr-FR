---
title: Activation du pipeline front-end pour l’archétype de projet AEM standard
description: Convertissez un projet d’AEM standard pour un déploiement plus rapide de ressources statiques telles que CSS, JavaScript, Polices et Icônes à l’aide du pipeline front-end. Et séparation du développement front-end et du développement back-end en pile complète sur AEM.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 2%

---


# Activation du pipeline front-end pour l’archétype de projet AEM standard{#enable-front-end-pipeline-standard-aem-project}

Découvrez comment activer le projet d’AEM standard créé à l’aide de [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype) pour déployer des ressources statiques telles que CSS, JavaScript, polices, icônes à l’aide du pipeline front-end afin d’accélérer le cycle de développement vers déploiement. Et séparation du développement front-end et du développement back-end en pile complète sur AEM. Vous découvrez également comment ces ressources statiques __not__ diffusé à partir du référentiel d’AEM, mais à partir du réseau de diffusion de contenu, un changement de paradigme de diffusion.

Un nouveau pipeline front-end est créé dans Adobe Cloud Manager qui ne crée et ne déploie que les éléments `ui.frontend` artefacts sur le réseau de diffusion de contenu et informations AEM sur son emplacement. Lors de la génération du HTML de la page web, la variable `<link>` et `<script>` les balises se rapportent à cet emplacement dans la variable `href` valeur d’attribut.

Après la conversion de projet d’AEM standard, les développeurs front-end peuvent travailler séparément et parallèlement à tout développement principal de pile complète sur AEM, qui possède ses propres pipelines de déploiement.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>Cela s’applique uniquement aux AEM as a Cloud Service et non aux déploiements Adobe Cloud Manager basés sur AMS.

