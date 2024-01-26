---
title: Tutoriel sur les bonnes pratiques pour Dynamic Media Classic
description: Dynamic Media Classic est le hub autour duquel les clientes et clients créent et diffusent du contenu multimédia riche. Ce tutoriel sur les bonnes pratiques a été créé pour aider les personnes expérimentées ou débutantes avec Dynamic Media Classic à mieux comprendre ce qu’elles peuvent faire avec cette solution performante de médias riches d’Adobe. Dans cette partie, vous découvrirez ce qu’est Dynamic Media Classic et examinerez brièvement ses principales fonctionnalités et son interface utilisateur.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 975b85af-ca6a-419e-ab2a-6e1781bfee4a
duration: 212
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 100%

---

# Tutoriel sur les bonnes pratiques pour Dynamic Media Classic

L’objectif de ce guide est d’aider les personnes expérimentées et débutantes avec Dynamic Media Classic à mieux comprendre ce qu’elles peuvent faire avec leur solution performante de médias riches d’Adobe. Nous atteindrons cet objectif en :

- présentant Dynamic Media Classic, en décrivant ses principales fonctionnalités et son interface utilisateur (IU) ;
- expliquant le workflow général de création et de diffusion que vous allez suivre lors de l’utilisation de ressources dans la solution ;
- argumentant sur les éléments importants à configurer avant de démarrer et d’utiliser la solution ;
- explorant l’utilisation de plusieurs des principales fonctionnalités de la solution.

Dans ce guide, nous vous donnerons des exemples, des conseils et des bonnes pratiques. Nous vous expliquerons également les termes et concepts importants que vous devez connaître lorsque vous utilisez Dynamic Media Classic. Et nous vous inviterons à consulter des webinaires, des articles de blog et de la documentation en ligne pertinents pour un sujet spécifique lorsque cela est possible.

Nous espérons que ce guide vous fournira les informations dont vous avez besoin pour tirer le meilleur parti de votre solution Dynamic Media Classic. Pour parcourir plus facilement les chapitres de ce guide, cliquez sur l’icône en forme de signet sur le côté gauche du guide pour en consulter le contenu.

## Présentation de Dynamic Media Classic

Dynamic Media Classic est le hub autour duquel les clientes et clients créent et diffusent du contenu multimédia riche. Dynamic Media Classic est un environnement intégré de gestion, de publication et de diffusion des médias riches. Les médias riches peuvent être diffusés sur tous les canaux marketing et de vente, y compris le web, les documents imprimés, les campagnes par e-mail, les applications web, les ordinateurs de bureau et les appareils.

La diffusion d’images est peut-être la fonction la plus utilisée de Dynamic Media Classic. En fait, la plupart des clientes et clients utilisent Dynamic Media Classic pour diffuser toutes les images sur leurs sites web, y compris les images pour le zoom ou les médias riches. Cependant, il peut également être utilisé à de nombreuses autres fins, notamment pour la diffusion de vidéos et l’utilisation de l’IA pour optimiser les images diffusées.

## Fonctionnalités principales de Dynamic Media Classic

Dans ce guide, nous aborderons les principales fonctionnalités suivantes de Dynamic Media Classic.

- **Dynamic Imaging.** : terme générique désignant la modification, le formatage et le dimensionnement en temps réel, ainsi que le zoom et le panoramique interactifs, les nuances de couleurs et de textures, la rotation à 360 degrés, les modèles d’image, et les visionneuses multimédia.
- **Vidéo.** : chargez les vidéos finales, publiez-les et téléchargez-les progressivement dans des visionneuses vidéo configurables.
- **Imagerie dynamique.** : technologie qui utilise les fonctionnalités de l’IA d’Adobe Sensei et les « paramètres d’image prédéfinis » existants pour améliorer les performances de la diffusion d’images en optimisant automatiquement le format, la taille et la qualité des images selon les fonctionnalités du navigateur client.

Pour découvrir d’autres fonctionnalités de la solution, rendez-vous sur [Documentation pour Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/intro/introduction.html?lang=fr).

## Interface utilisateur (IU) de Dynamic Media Classic

L’IU principale de Dynamic Media Classic se compose de trois zones principales : la barre de navigation générale, la bibliothèque de ressources et le panneau de navigation et de création.

![Image.](assets/overview/overview-dmc-ui-ew.png)

_IU de Dynamic Media Classic._

**Barre de navigation globale.** : vous pouvez utiliser les boutons de cette barre située en haut de l’écran pour accéder aux principales fonctionnalités et zones de la solution. Par exemple, vous l’utiliserez pour accéder aux fonctionnalités de chargement, ouvrir diverses zones de création de ressources (visionneuse d’images, visionneuse à 360°, etc.), exécuter des tâches importantes telles que la configuration des paramètres d’image prédéfinis et des paramètres de visionneuse prédéfinis, et publier vos ressources. Vous pouvez également surveiller vos tâches, voir les Activities récentes et choisir parmi diverses options d’aide.

**Bibliothèque de ressources.** : le panneau Bibliothèques de ressources est situé dans la partie gauche de l’écran et vous permet d’organiser vos ressources dans les dossiers et sous-dossiers que vous créez. Dans la partie supérieure du panneau, vous trouverez des filtres et des recherches pour vous aider à trouver les ressources. La recherche avancée (Advanced Search) vous permet de rechercher en spécifiant plusieurs options comme critères de recherche, y compris les champs de métadonnées masqués associés à cette ressource. Au bas du panneau, vous pouvez voir les éléments supprimés en cliquant sur l’icône Corbeille. Au départ, vous n’avez pas de dossiers, à l’exception du dossier de niveau supérieur, qui porte le même nom que votre nom de compte.

>[!NOTE]
>
>Les ressources dans la corbeille seront automatiquement supprimées de manière permanente 7 jours après les y avoir mis, sauf si vous les restaurez.

**Panneau naviguer et créer** : il s’agit du centre de l’IU, où vous pouvez parcourir les ressources en mode de navigation ou, où vous pouvez l’utiliser comme canevas pour créer des ressources dans le cadre d’un workflow en mode de création. Lorsque vous vous connectez pour la première fois, le panneau de navigation s’affiche. Au centre de l’écran se trouvent les versions miniatures de vos images dans une vue de grille. Vous pouvez changer la vue en liste ou sélectionner une ressource et en afficher les détails à l’aide de la vue Détails.

>[!IMPORTANT]
>
>À côté de chaque ID de ressource se trouve le bouton (bascule) **Marquer pour publication**. Lorsque le bouton (bascule) est activé (vert), il indique que la ressource est marquée pour publication.

![Image.](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>Cochez la case **Publier après le téléchargement** dans la boîte de dialogue de téléchargement pour publier automatiquement les ressources lors du téléchargement.

En savoir plus sur la [Navigation dans l’interface utilisateur de Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/navigation-basics.html?lang=fr).
