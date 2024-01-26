---
title: Créer un fragment de document
description: Bienvenue dans la partie 5 du tutoriel en plusieurs étapes sur la création de votre premier document de communication interactive. Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse de la personne destinataire.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 227
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 100%

---

# Créer un fragment de document

Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse de la personne destinataire.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Les fragments de document contiennent le contenu textuel des documents de communication interactive. Ce contenu de texte peut être du texte statique ou inséré à partir des valeurs des éléments du modèle de données sous-jacent. Par exemple **Cher _{name}_** : « Cher » est du texte statique et « name » est le nom de l’élément de modèle de données de formulaire. Au moment de l’exécution, cela donnera **Chère Gloria Rios**ou **Cher John Jacobs**selon la valeur de l’élément name.

L’éditeur de texte enrichi est suffisamment intuitif pour qu’une personne utilisatrice professionnelle puisse créer du texte et insérer des éléments de données de formulaire. L’éditeur de fragment de document permet de mettre en forme le texte, de spécifier les types et les styles de police, d’insérer des caractères spéciaux et de créer des hyperliens.

L’éditeur de fragment de document peut également insérer des conditions intégrées dans votre texte, comme le montre cette [video](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/create-document-fragment.html?lang=fr).

>[!NOTE]
>
>Assurez-vous que les éléments de modèle de données de formulaire que vous insérez dans un fragment de document sont des descendants de l’élément racine. Par exemple, dans ce cas d’utilisation, assurez-vous que les éléments de l’objet User que vous sélectionnez sont des éléments enfants de l’objet Balances.

## Étapes suivantes

[Créer un document de canal d’impression](./create-print-channel-document.md)
