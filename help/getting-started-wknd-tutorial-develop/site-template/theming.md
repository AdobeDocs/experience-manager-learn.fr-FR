---
title: Workflow de thème
seo-title: Prise en main d’AEM Sites - Workflow de thème
description: Découvrez comment mettre à jour les sources de thème d’un site Adobe Experience Manager pour appliquer des styles spécifiques à la marque. Découvrez comment utiliser un serveur proxy pour afficher un aperçu en direct des mises à jour CSS et JavaScript. Ce tutoriel explique également comment déployer des mises à jour de thème sur un site AEM à l’aide d’actions GitHub.
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Composants principaux
topic: Gestion de contenu, développement
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# Workflow de thème {#theming}

>[!CAUTION]
>
> Les fonctionnalités de création rapide de site présentées ici seront publiées au deuxième semestre 2021. La documentation associée est disponible à des fins d’aperçu.

Dans ce chapitre, nous allons mettre à jour les sources de thème d’un site Adobe Experience Manager pour appliquer des styles spécifiques à la marque. Nous allons apprendre à utiliser un serveur proxy pour afficher un aperçu des mises à jour CSS et JavaScript pendant que nous codons par rapport au site en direct. Ce tutoriel explique également comment déployer des mises à jour de thème sur un site AEM à l’aide d’actions GitHub.

À la fin, notre site sera mis à jour afin d’inclure des styles correspondant à la marque WKND.

## Prérequis {#prerequisites}

Ce tutoriel en plusieurs parties est supposé que les étapes décrites dans le chapitre [Modèles de page](./page-templates.md) ont été terminées.

## Objectifs

1. Découvrez comment télécharger et modifier les sources de thème d’un site.
1. Découvrez comment comparer le code au site en direct pour obtenir un aperçu en temps réel.
1. Comprenez le workflow de bout en bout de la diffusion de mises à jour CSS et JavaScript compilées dans le cadre d’un thème à l’aide des actions GitHub.

## Mettre à jour un thème {#theme-update}

Ensuite, apportez des modifications aux sources de thème afin que le site corresponde à l’apparence de la marque WKND.

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

Étapes de haut niveau pour la vidéo :

1. Créez un utilisateur local dans AEM à utiliser avec un serveur de développement proxy.
1. Téléchargez les sources de thèmes depuis AEM et ouvrez-les à l’aide d’un IDE local, comme VSCode.
1. Modifiez les sources des thèmes et utilisez un serveur de développement proxy pour prévisualiser les modifications CSS et JavaScript en temps réel.
1. Mettez à jour les sources du thème de sorte que l’article du magazine corresponde aux styles et aux maquettes de la marque WKND.

### Fichiers de solution

Téléchargez les styles terminés pour le [thème WKND](assets/theming/WKND-THEME-src.zip)

## Déployer un thème {#deploy-theme}

Déployez des mises à jour sur un thème dans un environnement AEM à l’aide des actions GitHub.

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

Étapes de haut niveau pour la vidéo :

1. Ajoutez votre projet de sources de thème à [GitHub en tant que nouveau référentiel](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line).
1. Créez [un jeton d’accès personnel dans GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) et enregistrez-le à un emplacement sécurisé.
1. Configurez les paramètres GitHub dans votre environnement AEM pour pointer vers votre référentiel GitHub et inclure votre jeton d’accès personnel.
1. Créez les [secrets chiffrés](https://docs.github.com/en/actions/reference/encrypted-secrets) suivants dans votre référentiel GitHub :
   * **AEM_SITE**  : racine de votre site AEM (c.-à-d.  `wknd`)
   * **AEM_URL**  : URL de votre environnement de création AEM
   * **GIT_TOKEN**  - Jeton d’accès personnel de l’étape 2.
1. Exécutez l’action GitHub : **Créez et déployez des artefacts Github**. Cela aura l’effet en aval de l’exécution de l’action : **Mettez à jour la configuration du thème sur AEM avec l’artefact id**, qui déploie les sources du thème dans l’environnement AEM comme spécifié par `AEM_URL` et `AEM_SITE`.

### Exemple de référentiels

Il existe quelques exemples de repos GitHub qui peuvent être utilisés comme référence :

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  : utilisé comme exemple pour les projets &quot;réels&quot;.
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  - Utilisé comme exemple pour ceux qui suivent le tutoriel.

## Félicitations !  {#congratulations}

Félicitations, vous venez de créer une mise à jour et de déployer un thème pour AEM !

### Étapes suivantes {#next-steps}

Explorez plus en détail le développement AEM et comprenez mieux la technologie sous-jacente en créant un site à l’aide de l’[AEM archétype de projet](../project-archetype/overview.md).
