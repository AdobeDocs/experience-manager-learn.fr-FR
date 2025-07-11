---
title: Workflow de thème | Création rapide de site AEM
description: Découvrez comment mettre à jour les sources de thème d’un site Adobe Experience Manager pour appliquer des styles spécifiques à la marque. Découvrez comment utiliser un serveur proxy pour afficher un aperçu en direct des mises à jour CSS et JavaScript. Ce tutoriel explique également comment déployer des mises à jour de thème sur un site AEM à l’aide du pipeline front-end d’Adobe Cloud Manager.
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# Workflow de thème {#theming}

Dans ce chapitre, nous mettons à jour les sources de thème d’un site Adobe Experience Manager pour appliquer des styles spécifiques à la marque. Nous apprenons à utiliser un serveur proxy pour visualiser un aperçu des mises à jour CSS et JavaScript pendant que nous codons sur le site en direct. Ce tutoriel explique également comment déployer des mises à jour de thème sur un site AEM à l’aide du pipeline front-end d’Adobe Cloud Manager.

Finalement, notre site est mis à jour pour inclure des styles correspondant à la marque WKND.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans le chapitre [Modèles de page](./page-templates.md) ont été terminées.

## Objectifs

1. Découvrez comment télécharger et modifier les sources de thème d’un site.
1. Découvrez comment coder sur le site en direct pour obtenir un aperçu en temps réel.
1. Comprenez le workflow de bout en bout de la livraison de mises à jour compilées de CSS et JavaScript dans le cadre d’un thème à l’aide du pipeline front-end d’Adobe Cloud Manager.

## Mettre à jour un thème {#theme-update}

Ensuite, apportez des modifications aux sources de thème afin que le site corresponde à l’aspect de la marque WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3453622?quality=12&learn=on&captions=fre_fr)

Étapes de haut niveau pour la vidéo :

1. Créez un utilisateur local ou une utilisatrice locale dans AEM à utiliser avec un serveur de développement proxy.
1. Téléchargez les sources de thème depuis AEM et ouvrez-les à l’aide d’un IDE local, comme VSCode.
1. Modifiez les sources de thème et utilisez un serveur de développement proxy pour prévisualiser les modifications CSS et JavaScript en temps réel.
1. Mettez à jour les sources de thème de sorte que l’article du magazine corresponde aux styles et aux maquettes de la marque WKND.

### Fichiers de solution

Téléchargez les styles terminés pour l’[exemple de thème WKND](assets/theming/WKND-THEME-src-1.1.zip).

## Déployer un thème à l’aide d’un pipeline front-end {#deploy-theme}

Déployez des mises à jour sur un thème dans un environnement AEM à l’aide du pipeline front-end de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

Étapes de haut niveau pour la vidéo :

1. Créez un nouveau [référentiel git dans Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/managing-code/repositories.html?lang=fr).
1. Ajoutez votre projet de sources de thème au référentiel git de Cloud Manager :

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. Configurez un [Pipeline front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr) dans Cloud Manager pour déployer le code front-end.
1. Exécutez le pipeline front-end pour déployer des mises à jour dans l’environnement AEM cible.

### Exemple de référentiels

Il existe quelques exemples de référentiels GitHub qui peuvent être utilisés comme référence :

* [aem-site-template-standard.](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-standard-theme-e2e) - Utilisé comme exemple pour les projets « en situation réelle ».

## Félicitations. {#congratulations}

Félicitations, vous venez de mettre à jour et de déployer un thème sur AEM.

### Étapes suivantes {#next-steps}

Découvrez plus en détail le développement AEM et comprenez mieux la technologie sous-jacente en créant un site à l’aide de l’[Archétype de projet AEM](../project-archetype/overview.md).
