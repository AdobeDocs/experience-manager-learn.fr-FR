---
title: Création de fragments de Document pour contenir le nom et l’adresse du destinataire
seo-title: Création de fragments de Document pour contenir le nom et l’adresse du destinataire
description: 'Voici la partie 5 d''un didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous allons créer un fragment de document pour contenir le nom et l’adresse du destinataire. '
seo-description: 'Voici la partie 5 d''un didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous allons créer un fragment de document pour contenir le nom et l’adresse du destinataire. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---


# Création de fragments de Document pour contenir le nom et l’adresse du destinataire {#creating-document-fragments-to-hold-the-recipient-name-and-address}

Dans cette partie, nous allons créer un fragment de document pour contenir le nom et l’adresse du destinataire.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Les fragments de document contiennent le contenu textuel des documents de communication interactifs. Ce contenu de texte peut être du texte statique ou inséré à partir des valeurs des éléments de modèle de données sous-jacentes. Par exemple, Cher {name}, où Cher est du texte statique et {name} est le nom de l’élément de données du formulaire. Au moment de l’exécution, cette résolution sera transmise à Chère Gloria Rios ou Chère John Jacobs selon la valeur de l’élément name.

L’éditeur de texte enrichi est suffisamment intuitif pour qu’un utilisateur puisse créer du texte et insérer des éléments de données de formulaire. L’éditeur de fragments de document permet de mettre en forme du texte, de spécifier les types et les styles de police, d’insérer des caractères spéciaux et de créer des hyperliens.

L’éditeur de fragments de document peut également insérer des conditions en ligne dans votre texte, comme le montre cette [vidéo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assurez-vous que les éléments de modèle de données de formulaire que vous insérez dans un fragment de document sont des descendants de l’élément racine. Par exemple, dans ce cas d&#39;utilisation, assurez-vous que les éléments de l&#39;objet Utilisateur que vous sélectionnez sont l&#39;enfant de l&#39;objet Balance

