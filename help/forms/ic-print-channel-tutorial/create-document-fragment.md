---
title: Création d’un fragment de document
description: Ce didacticiel en plusieurs étapes est la cinquième partie d’un premier document de communication interactive. Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse du destinataire.
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 3%

---

# Création d’un fragment de document

Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse du destinataire.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Les fragments de document contiennent le contenu textuel des documents de communication interactive. Ce contenu de texte peut être du texte statique ou inséré à partir des valeurs des éléments de modèle de données sous-jacentes. Par exemple **Cher _{name}_**, où Cher est du texte statique et nom est le nom de l’élément de modèle de données de formulaire. Au moment de l’exécution, la résolution est **Chère Gloria Rios**ou **Cher John Jacobs**selon la valeur de l’élément name .

L’éditeur de texte enrichi est suffisamment intuitif pour qu’un utilisateur chargé de la conception de formulaires puisse créer du texte et insérer des éléments de données de formulaire. L’éditeur de fragment de document permet de mettre en forme le texte, de spécifier les types et les styles de police, d’insérer des caractères spéciaux et de créer des hyperliens.

L’éditeur de fragment de document peut également insérer des conditions en ligne dans votre texte, comme le montre cette section. [video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assurez-vous que les éléments de modèle de données de formulaire que vous insérez dans un document sont des descendants de l’élément racine. Par exemple, dans ce cas pratique, assurez-vous que les éléments de l’objet utilisateur que vous sélectionnez sont l’enfant de l’objet soldes .

## Étapes suivantes

[Créer un document de canal d’impression](./create-print-channel-document.md)
