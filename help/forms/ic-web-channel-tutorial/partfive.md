---
title: Créer des fragments de document pour contenir le nom et l’adresse de la personne destinataire
description: Bienvenue dans la partie 5 du tutoriel en plusieurs étapes sur la création de votre premier document de communication interactive. Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse de la personne destinataire.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 229
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 100%

---

# Créer des fragments de document pour contenir le nom et l’adresse de la personne destinataire {#creating-document-fragments-to-hold-the-recipient-name-and-address}

Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse de la personne destinataire.

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

Les fragments de document contiennent le contenu textuel des documents de communication interactive. Ce contenu de texte peut être du texte statique ou inséré à partir des valeurs des éléments du modèle de données sous-jacent. Par exemple, Cher {name}, où Cher est du texte statique et {name} est le nom de l’élément de données de formulaire. Au moment de l’exécution, cette opération sera résolue sur Chère Gloria Rios ou Cher John Jacobs selon la valeur de l’élément name.

L’éditeur de texte enrichi est suffisamment intuitif pour qu’une personne utilisatrice professionnelle puisse créer du texte et insérer des éléments de données de formulaire. L’éditeur de fragment de document permet de mettre en forme le texte, de spécifier les types et les styles de police, d’insérer des caractères spéciaux et de créer des hyperliens.

L’éditeur de fragment de document permet également d’insérer des conditions intégrées dans votre texte, comme illustré dans cette [vidéo](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/create-document-fragment.html?lang=fr).

>[!NOTE]
>
>Assurez-vous que les éléments de modèle de données de formulaire que vous insérez dans un fragment de document sont des descendants de l’élément racine. Par exemple, dans ce cas d’utilisation, assurez-vous que les éléments de l’objet User que vous sélectionnez sont des éléments enfants de l’objet Balances.

## Étapes suivantes

[Créer un document de communication interactive](./partsix.md)