---
title: Flux de travaux de thème
seo-title: Prise en main de AEM Sites - Flux de travaux de thème
description: Découvrez comment mettre à jour les sources de thème d'un site Adobe Experience Manager pour appliquer des styles spécifiques à la marque. Découvrez comment utiliser un serveur proxy pour vue d’une prévisualisation active de mises à jour CSS et Javascript. Ce didacticiel explique également comment déployer des mises à jour de thème sur un site AEM à l’aide des actions GitHub.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Composants principaux
topic: gestion de contenu, développement
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# Flux de travaux de thème {#theming}

>[!CAUTION]
>
> Les fonctions de création rapide du site présentées ici seront publiées au second semestre 2021. La documentation correspondante est disponible à des fins de prévisualisation.

Dans ce chapitre, nous allons mettre à jour les sources de thème d&#39;un site Adobe Experience Manager pour appliquer des styles spécifiques à la marque. Nous apprendrons à utiliser un serveur proxy pour vue une prévisualisation de mises à jour CSS et Javascript pendant que nous codons par rapport au site actif. Ce didacticiel explique également comment déployer des mises à jour de thème sur un site AEM à l’aide des actions GitHub.

En fin de compte, notre site sera mis à jour pour inclure des styles correspondant à la marque WKND.

## Conditions préalables {#prerequisites}

Il s&#39;agit d&#39;un didacticiel en plusieurs parties et on suppose que les étapes décrites dans le chapitre [Modèles de page](./page-templates.md) ont été terminées.

## Objectifs

1. Découvrez comment télécharger et modifier les sources de thème d&#39;un site.
1. Découvrez comment utiliser le code par rapport au site en direct pour une prévisualisation en temps réel.
1. Comprenez le processus complet de diffusion de mises à jour CSS et JavaScript compilées dans le cadre d’un thème à l’aide des actions GitHub.

## Mettre à jour un thème {#theme-update}

Ensuite, apportez des modifications aux sources du thème afin que le site corresponde à l’apparence de la marque WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Etapes de haut niveau pour la vidéo :

1. Créez un utilisateur local dans AEM à utiliser avec un serveur de développement proxy.
1. Téléchargez les sources du thème depuis AEM et ouvrez-les à l&#39;aide d&#39;un IDE local, comme VSCode.
1. Modifiez les sources du thème et utilisez un serveur de développement proxy pour prévisualisation les modifications CSS et JavaScript en temps réel.
1. Mettez à jour les sources du thème afin que l’article du magazine corresponde aux styles et maquettes de marque WKND.

### Fichiers de solution

Téléchargez les styles terminés pour le thème [WKND](assets/theming/WKND-THEME-src.zip).

## Déployer un thème {#deploy-theme}

Déployez des mises à jour sur un thème vers un environnement AEM à l’aide des actions GitHub.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Etapes de haut niveau pour la vidéo :

1. Ajoutez votre projet de sources de thème sur [GitHub en tant que nouveau référentiel](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line).
1. Créez [un jeton d&#39;accès personnel dans GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) et enregistrez-le dans un emplacement sécurisé.
1. Configurez les paramètres GitHub dans votre environnement AEM pour qu’ils pointent vers votre référentiel GitHub et incluent votre jeton d&#39;accès personnel.
1. Créez les [secrets chiffrés](https://docs.github.com/en/actions/reference/encrypted-secrets) suivants dans votre référentiel GitHub :
   * **AEM_SITE**  : racine de votre site AEM (c.-à-d.  `wknd`)
   * **AEM_URL** - URL de votre environnement d’auteur AEM
   * **GIT_TOKEN**  - jeton d&#39;accès personnel de l&#39;étape 2.
1. Exécutez l&#39;action GitHub : **Créer et déployer des artefacts Github**. Cela aura pour effet en aval d’exécuter l’action : **Mettre à jour la configuration du thème sur AEM avec l&#39;artefact id**, qui déploiera les sources du thème vers l&#39;environnement AEM comme spécifié par `AEM_URL` et `AEM_SITE`.

### Exemple de repos

Il existe quelques exemples de repos GitHub qui peuvent être utilisés comme référence :

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - Utilisé comme exemple pour les projets &quot;réels&quot;.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme) - Utilisé comme exemple pour ceux qui suivent le didacticiel.

## Félicitations! {#congratulations}

Félicitations, vous venez de créer une mise à jour et de déployer un thème pour AEM !

### Étapes suivantes {#next-steps}

Approfondissez le développement des AEM et mieux comprendre la technologie sous-jacente en créant un site à l&#39;aide de l&#39;[AEM Project Archetype](../project-archetype/overview.md).
