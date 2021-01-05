---
title: Bienvenue dans le didacticiel des meilleures pratiques de Dynamic Media Classic
description: Dynamic Media Classic est le hub autour duquel les clients créent, créent et diffusent du contenu multimédia enrichi. Ce tutoriel des meilleures pratiques a été créé pour aider les utilisateurs actuels et nouveaux de Dynamic Media Classic à mieux comprendre ce qu'ils peuvent faire avec cette puissante solution de média enrichi à partir d'Adobe. Dans cette partie du didacticiel, vous découvrirez ce qu’est Dynamic Media Classic et vous obtiendrez un bref aperçu de ses principales fonctionnalités et de son interface utilisateur.
sub-product: dynamic-media
doc-type: tutorial
topics: best-practices, development, authoring, configuring
audience: all
activity: develop, use
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 1%

---


# Bienvenue dans le didacticiel des meilleures pratiques de Dynamic Media Classic

Ce guide est destiné à aider les utilisateurs actuels et nouveaux de Dynamic Media Classic à mieux comprendre ce qu&#39;ils peuvent faire avec leur puissante solution de média enrichi à partir de l&#39;Adobe. Nous le ferons en :

- Présentation de Dynamic Media Classic, description de ce qu’il est et présentation de ses principales fonctionnalités et de son interface utilisateur.
- Explication du flux de travaux général de création, d’auteur et de diffusion que vous suivrez lors de l’utilisation de ressources dans la solution.
- Discuter des éléments importants à configurer avant d’accéder à la solution et de l’utiliser.
- Approfondir l&#39;analyse pour utiliser plusieurs des principales fonctionnalités de la solution.

Tout au long du guide, nous vous donnerons des exemples, des conseils et des bonnes pratiques. Nous vous expliquerons également des termes et concepts importants que vous devez connaître lorsque vous travaillez avec Dynamic Media Classic. Et lorsqu&#39;il sera disponible pour un sujet donné, nous vous indiquerons les webinaires, les billets de blog et la documentation en ligne pertinents.

Nous espérons que ce guide vous fournira les informations dont vous avez besoin pour déverrouiller une valeur exceptionnelle de votre solution Dynamic Media Classic. Pour parcourir plus facilement les chapitres de ce guide, cliquez sur l&#39;icône en forme de signet située à gauche du guide pour en afficher le contenu.

## Présentation de Dynamic Media Classic

Dynamic Media Classic est le hub autour duquel les clients créent, créent et diffusent du contenu multimédia enrichi. Dynamic Media Classic est un environnement intégré de gestion, de publication et de diffusion des médias enrichis. Les médias enrichis peuvent être diffusés sur tous les canaux de marketing et de vente, y compris le Web, le matériel imprimé, les campagnes par courrier électronique, les applications Web, les ordinateurs de bureau et les périphériques.

La diffusion d’images est peut-être la fonction la plus utilisée de Dynamic Media Classic. En fait, la plupart des clients utilisent Dynamic Media Classic pour afficher toutes les images de leur site Web, y compris les images destinées au zoom ou aux médias enrichis. Cependant, il peut également être utilisé à de nombreuses autres fins, y compris la diffusion de vidéos et l&#39;utilisation de l&#39;IA pour optimiser les images diffusées.

## Fonctionnalités de base de Dynamic Media Classic

Ce guide porte sur les principales fonctionnalités suivantes de Dynamic Media Classic.

- **Imagerie dynamique.** terme générique désignant la modification, le formatage et le dimensionnement en temps réel, ainsi que le zoom et le panoramique interactifs ; échantillonnage de couleur et de texture ; spin à 360 degrés; modèles d&#39;image ; et les visionneuses multimédia.
- **Vidéo.** Téléchargez les vidéos finales, publiez-les et téléchargez-les progressivement dans des visionneuses de vidéos configurables.
- **Imagerie dynamique.** Technologie qui exploite les fonctionnalités d’IA Adobe Sensei et qui fonctionne avec les &quot;paramètres d’image prédéfinis&quot; existants pour améliorer les performances de la diffusion d’images en optimisant automatiquement le format, la taille et la qualité d’image en fonction des fonctionnalités du navigateur client.

Pour découvrir d&#39;autres fonctionnalités de la solution, consultez la [Documentation pour Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/intro/introduction.html).

## Interface utilisateur de Dynamic Media Classic

L’interface utilisateur principale de Dynamic Media Classic se compose de trois zones principales : la barre de navigation globale, la bibliothèque de fichiers et le panneau de navigation/de création.

![image](assets/overview/overview-dmc-ui-ew.png)

_Interface utilisateur Dynamic Media Classic_

**Barre de navigation globale.** Situés en haut de votre écran, vous utiliserez les boutons de cette barre pour accéder aux zones clés et aux fonctionnalités de la solution. Par exemple, vous l’utiliserez pour accéder aux fonctionnalités de téléchargement, ouvrir diverses zones de création de fichiers (visionneuse d’images, visionneuse à 360°, etc.), effectuer des tâches importantes telles que la configuration des paramètres d’image prédéfinis et des paramètres prédéfinis de la visionneuse, et publier vos fichiers. Vous pouvez également surveiller vos tâches, consulter les activités récentes et choisir parmi diverses options d’aide.

**Bibliothèque de ressources.** La bibliothèque de fichiers se trouve dans le coin inférieur gauche de l’écran. Elle est un panneau que vous utilisez pour organiser vos fichiers dans des dossiers et des sous-dossiers que vous créez. En haut du panneau, vous trouverez des filtres et des recherches pour vous aider à localiser des fichiers. La recherche avancée vous permet de rechercher en spécifiant plusieurs options comme critères de recherche, y compris les champs de métadonnées masqués attachés à cette ressource. Au bas du panneau, vous pouvez voir les éléments supprimés en cliquant sur l’icône Corbeille. Au départ, vous ne début avec aucun dossier, à l’exception du dossier de niveau supérieur, qui porte le même nom que le nom de votre compte.

>[!NOTE]
>
>Les fichiers de la corbeille seront automatiquement supprimés définitivement sept jours après leur mise en place, sauf si vous les restaurez.

**Panneau de navigation/création.** Il s’agit du centre de l’interface utilisateur, où vous allez soit parcourir les fichiers en mode Navigation, soit, si vous êtes en mode Création, vous l’utiliserez comme trame pour créer des ressources dans le cadre d’un processus. La première fois que vous vous connectez, le panneau de navigation s’affiche. Au centre de l’écran se trouvent des versions miniatures de vos images dans une vue de grille. Vous pouvez passer à une vue de Liste ou sélectionner un fichier et des détails de vue à son sujet à l’aide de la vue de détails.

>[!IMPORTANT]
>
>À côté de chaque ID de ressource se trouve le commutateur **Marquer pour publication**. Lorsque la bascule est activée (verte), cela indique que la ressource est marquée pour publication.

![image](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>Cochez la case **Publier après le téléchargement** dans la boîte de dialogue Télécharger pour publier automatiquement les fichiers au moment du téléchargement.

En savoir plus sur [la navigation dans l’interface utilisateur de Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/getting-started/navigation-basics.html).
