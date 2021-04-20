---
title: Utilisation de la traduction de fichiers source avec AEM Assets
description: Les ressources Adobe Experience Manager (AEM) vous permettent d’identifier les ressources qui partagent des attributs communs et de les marquer comme liées à l’aide de la nouvelle fonction Ressources connexes. Il permet également aux utilisateurs de définir une relation source/dérivée entre les ressources, ce qui permet aux utilisateurs d’identifier facilement l’origine d’une ressource. L’exécution du processus de traduction sur une ressource dérivée récupère toute ressource référencée par le fichier source et l’inclut pour la traduction, réduisant ainsi les efforts de maintenance de plusieurs sites.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---


# Utilisation de la traduction de fichiers source avec AEM Assets {#using-source-file-translation-with-aem-assets}

Les ressources Adobe Experience Manager (AEM) vous permettent d’identifier les ressources qui partagent des attributs communs et de les marquer comme liées à l’aide de la nouvelle fonction Ressources connexes. Il permet également aux utilisateurs de définir une relation source/dérivée entre les ressources, ce qui permet aux utilisateurs d’identifier facilement l’origine d’une ressource. L’exécution du processus de traduction sur une ressource dérivée récupère toute ressource référencée par le fichier source et l’inclut pour la traduction, réduisant ainsi les efforts de maintenance de plusieurs sites.

## Gestion des fichiers source multisite {#multisite-asset-source-file-management}

>[!VIDEO](https://video.tv.adobe.com/v/18331/?quality=9&learn=on)

Les ressources connexes aident les utilisateurs à mieux gérer les ressources liées entre elles avec des caractéristiques, des propriétés partagées et à rationaliser les workflows :

* Nouvelle fonctionnalité Ressources connexes permettant de relier manuellement des ressources présentant des caractéristiques similaires ou appartenant à une même campagne ou projet
* L’utilisateur peut vue des fichiers associés pour une ressource sous Propriétés de la vue. Un utilisateur peut accéder aux fichiers associés à partir de la fenêtre des propriétés de la vue.
* Si les propriétés de deux éléments connexes ont changé, les utilisateurs peuvent annuler la relation entre ces éléments à l’aide de l’option Annuler la relation.
* Lorsque vous essayez de supprimer une ressource associée, un message d’avertissement s’affiche si elle contient d’autres ressources connexes.
* L’exécution d’un processus de traduction sur une ressource liée dérivée ajoute les fichiers source connexes au processus de traduction, ce qui facilite la gestion multisite.