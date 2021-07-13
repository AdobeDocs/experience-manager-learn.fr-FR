---
title: Bienvenue dans le tutoriel des bonnes pratiques de Dynamic Media Classic
description: Dynamic Media Classic est le hub autour duquel les clients créent, créent et diffusent du contenu multimédia. Ce tutoriel sur les bonnes pratiques a été créé pour aider les utilisateurs actuels et nouveaux de Dynamic Media Classic à mieux comprendre ce qu’ils peuvent faire avec cette puissante solution de médias riches d’Adobe. Dans cette partie du tutoriel, vous découvrirez ce qu’est Dynamic Media Classic et examinerez brièvement ses principales fonctionnalités et son interface utilisateur.
sub-product: dynamic-media
doc-type: tutorial
topics: best-practices, development, authoring, configuring
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: Gestion de contenu
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 1%

---


# Bienvenue dans le tutoriel des bonnes pratiques de Dynamic Media Classic

L’objectif de ce guide est d’aider les utilisateurs actuels et nouveaux de Dynamic Media Classic à mieux comprendre ce qu’ils peuvent faire avec leur puissante solution de médias riches d’Adobe. Nous allons le faire en :

- Présentation de Dynamic Media Classic, description de ce qu’il est et présentation de ses principales fonctionnalités et de son interface utilisateur.
- Expliquer le workflow général Créer, créer et diffuser que vous allez suivre lors de l’utilisation de ressources dans la solution.
- Discussions sur les éléments importants à configurer avant de passer à la solution et de l’utiliser.
- Découvrez comment utiliser plusieurs des principales fonctionnalités de la solution.

Dans ce guide, nous vous donnerons des exemples, des conseils et des bonnes pratiques. Nous vous expliquerons également les termes et concepts importants que vous devez connaître lorsque vous utilisez Dynamic Media Classic. Et lorsqu’il sera disponible pour un sujet donné, nous vous invitons à consulter des webinaires, des billets de blog et de la documentation en ligne pertinents.

Nous espérons que ce guide vous fournira les informations dont vous avez besoin pour déverrouiller une valeur exceptionnelle de votre solution Dynamic Media Classic. Pour parcourir plus facilement les chapitres de ce guide, cliquez sur l’icône en forme de signet sur le côté gauche du guide pour en consulter le contenu.

## Présentation de Dynamic Media Classic

Dynamic Media Classic est le hub autour duquel les clients créent, créent et diffusent du contenu multimédia. Dynamic Media Classic est un environnement intégré de gestion, de publication et de service des médias riches. Les médias riches peuvent être diffusés sur tous les canaux marketing et de vente, y compris le web, le matériel d’impression, les campagnes par e-mail, les applications web, les ordinateurs de bureau et les appareils.

La diffusion d’images est peut-être la fonction la plus utilisée de Dynamic Media Classic. En fait, la plupart des clients utilisent Dynamic Media Classic pour diffuser toutes les images de leurs sites web, y compris les images pour le zoom ou les médias riches. Cependant, il peut également être utilisé à de nombreuses autres fins, notamment pour la diffusion de vidéos et l’utilisation de l’IA pour optimiser les images diffusées.

## Fonctionnalités principales de Dynamic Media Classic

Dans ce guide, nous aborderons les principales fonctionnalités suivantes de Dynamic Media Classic.

- **Imagerie dynamique.** Terme générique désignant la modification, le formatage et le dimensionnement en temps réel, ainsi que le zoom et le panoramique interactifs ; nuances de couleurs et de textures ; rotation à 360 degrés; modèles d’image ; et les visionneuses multimédia.
- **Vidéo.** Chargez les vidéos finales, publiez-les et téléchargez-les progressivement dans des visionneuses vidéo configurables.
- **Imagerie dynamique.** Technologie qui tire parti des fonctionnalités d’Adobe Sensei AI et qui fonctionne avec les &quot;paramètres d’image prédéfinis&quot; existants pour améliorer les performances de diffusion d’images en optimisant automatiquement le format, la taille et la qualité des images en fonction des fonctionnalités du navigateur client.

Pour découvrir d’autres fonctionnalités de la solution, consultez la [Documentation pour Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/intro/introduction.html).

## Interface utilisateur de Dynamic Media Classic

L’interface utilisateur principale de Dynamic Media Classic se compose de trois zones principales : La barre de navigation globale, la bibliothèque de ressources et le panneau de navigation/de création.

![image](assets/overview/overview-dmc-ui-ew.png)

_Interface utilisateur de Dynamic Media Classic_

**Barre de navigation globale.** Les boutons de cette barre situés en haut de l’écran vous permettront d’accéder aux principales fonctionnalités de la solution. Par exemple, vous l’utiliserez pour accéder aux fonctionnalités de chargement, ouvrir diverses zones de création de ressources (visionneuse d’images, visionneuse à 360°, etc.), exécuter des tâches importantes telles que la configuration des paramètres d’image prédéfinis et des paramètres de visionneuse prédéfinis, et publier vos ressources. Vous pouvez également surveiller vos tâches, voir les activités récentes et choisir parmi diverses options d’aide.

**Bibliothèque de ressources.** Dans la partie gauche de l’écran se trouve la bibliothèque de ressources, un panneau que vous utilisez pour organiser vos ressources dans les dossiers et sous-dossiers que vous créez. Dans la partie supérieure du panneau, vous trouverez des filtres et des recherches pour vous aider à localiser les ressources. La recherche avancée vous permet de rechercher en spécifiant plusieurs options comme critères de recherche, y compris les champs de métadonnées masqués associés à cette ressource. Au bas du panneau, vous pouvez voir les éléments supprimés en cliquant sur l’icône Corbeille . Au départ, vous ne commencez par aucun dossier, à l’exception du dossier de niveau supérieur, qui porte le même nom que votre nom de compte.

>[!NOTE]
>
>Les ressources de la corbeille seront automatiquement supprimées sept jours après leur mise à disposition, sauf si vous les restaurez.

**Parcourir/créer le panneau.** Il s’agit du centre de l’interface utilisateur, où vous pouvez parcourir les ressources en mode Navigation ou, en mode Création, vous pouvez l’utiliser comme canevas pour créer des ressources dans le cadre d’un workflow. Lorsque vous vous connectez pour la première fois, le panneau de navigation s’affiche. Au centre de l’écran se trouvent les versions miniatures de vos images en mode Grille. Vous pouvez passer en mode Liste ou sélectionner une ressource et en afficher les détails à l’aide de la vue Détails.

>[!IMPORTANT]
>
>À côté de chaque ID de ressource se trouve le commutateur **Marquer pour publication**. Lorsque le bouton bascule est activé (vert), il indique que la ressource est marquée pour publication.

![image](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>Cochez la case **Publier après le téléchargement** dans la boîte de dialogue Télécharger pour publier automatiquement les ressources lors du téléchargement.

En savoir plus sur [la navigation dans l’interface utilisateur de Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/getting-started/navigation-basics.html).
