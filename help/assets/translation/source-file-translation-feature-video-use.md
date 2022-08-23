---
title: Utilisation de la traduction de fichiers source avec AEM Assets
description: Adobe Experience Manager (AEM) Assets vous permet d’identifier les ressources qui partagent des attributs communs et de les marquer comme liées à l’aide de la nouvelle fonctionnalité Ressources liées . Il permet également aux utilisateurs de définir une relation source/dérivée entre les ressources, ce qui facilite l’identification de l’origine d’une ressource. L’exécution du workflow de traduction sur une ressource dérivée récupère toute ressource référencée par le fichier source et l’inclut pour traduction, réduisant ainsi les efforts de maintenance de sites multiples.
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: 19155d51-18a7-42ca-9a73-1b5fa1355d58
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# Utilisation de la traduction de fichiers source avec AEM Assets {#using-source-file-translation-with-aem-assets}

Adobe Experience Manager (AEM) Assets vous permet d’identifier les ressources qui partagent des attributs communs et de les marquer comme liées à l’aide de la nouvelle fonctionnalité Ressources liées . Il permet également aux utilisateurs de définir une relation source/dérivée entre les ressources, ce qui facilite l’identification de l’origine d’une ressource. L’exécution du workflow de traduction sur une ressource dérivée récupère toute ressource référencée par le fichier source et l’inclut pour traduction, réduisant ainsi les efforts de maintenance de sites multiples.

## Gestion de fichiers source de ressources multisite {#multisite-asset-source-file-management}

>[!VIDEO](https://video.tv.adobe.com/v/18331/?quality=9&learn=on)

Les ressources connexes aident les utilisateurs à mieux gérer les ressources interconnectées avec des caractéristiques partagées, des propriétés et à rationaliser les workflows :

* Nouvelle fonctionnalité Ressources liées pour relier manuellement des ressources présentant des caractéristiques similaires ou appartenant à la même campagne ou au même projet
* L’utilisateur peut afficher les fichiers associés d’une ressource sous Afficher les propriétés. Un utilisateur peut accéder aux fichiers associés à partir de la fenêtre des propriétés d’affichage.
* Si les propriétés de deux ressources connexes ont été modifiées, les utilisateurs peuvent annuler la relation entre ces ressources à l’aide de l’option Ne plus mettre en relation .
* Lorsque vous essayez de supprimer une ressource associée, un message d’avertissement s’affiche si elle contient d’autres ressources associées.
* L’exécution d’un workflow de traduction sur une ressource liée dérivée ajoute les fichiers source associés au workflow de traduction, ce qui facilite la gestion multisite.
