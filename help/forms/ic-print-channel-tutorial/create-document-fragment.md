---
title: Création d’un fragment de Document
description: 'Voici la partie 5 d''un didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous allons créer un fragment de document pour contenir le nom et l’adresse du destinataire. '
seo-description: 'Voici la partie 5 d''un didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous allons créer un fragment de document pour contenir le nom et l’adresse du destinataire. '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# Création d’un fragment de Document

Dans cette partie, nous allons créer un fragment de document pour contenir le nom et l’adresse du destinataire.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Les fragments de document contiennent le contenu textuel des documents de communication interactifs. Ce contenu de texte peut être du texte statique ou inséré à partir des valeurs des éléments de modèle de données sous-jacentes. Par exemple **Cher _{name}_**, où Cher est du texte statique et où nom est le nom de l’élément de modèle de données de formulaire. Au moment de l’exécution, la résolution sera **Cher Gloria Rios**ou **Cher John Jacobs**selon la valeur de l’élément name.

L’éditeur de texte enrichi est suffisamment intuitif pour qu’un utilisateur puisse créer du texte et insérer des éléments de données de formulaire. L’éditeur de fragments de document permet de mettre en forme du texte, de spécifier les types et les styles de police, d’insérer des caractères spéciaux et de créer des hyperliens.

L’éditeur de fragments de document peut également insérer des conditions en ligne dans votre texte, comme le montre cette [vidéo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assurez-vous que les éléments de modèle de données de formulaire que vous insérez dans un fragment de document sont des descendants de l’élément racine. Par exemple, dans ce cas d&#39;utilisation, assurez-vous que les éléments de l&#39;objet Utilisateur que vous sélectionnez sont l&#39;enfant de l&#39;objet Balance

