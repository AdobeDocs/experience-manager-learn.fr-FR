---
title: Création de fragments de document pour contenir le nom et l’adresse du destinataire
seo-title: Création de fragments de document pour contenir le nom et l’adresse du destinataire
description: 'Ce didacticiel en plusieurs étapes est la cinquième partie d’un premier document de communication interactive. Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse du destinataire. '
seo-description: 'Ce didacticiel en plusieurs étapes est la cinquième partie d’un premier document de communication interactive. Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse du destinataire. '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Création de fragments de document pour contenir le nom et l’adresse du destinataire {#creating-document-fragments-to-hold-the-recipient-name-and-address}

Dans cette partie, nous allons créer un fragment de document qui contiendra le nom et l’adresse du destinataire.

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

Les fragments de document contiennent le contenu textuel des documents de communication interactive. Ce contenu de texte peut être du texte statique ou inséré à partir des valeurs des éléments de modèle de données sous-jacentes. Par exemple, Dear {name}, où Dear est du texte statique et {name} est le nom de l’élément de données de formulaire. Au moment de l’exécution, cette opération sera résolue sur Cher Gloria Rios ou Cher John Jacobs selon la valeur de l’élément name .

L’éditeur de texte enrichi est suffisamment intuitif pour qu’un utilisateur chargé de la conception de formulaires puisse créer du texte et insérer des éléments de données de formulaire. L’éditeur de fragment de document permet de mettre en forme le texte, de spécifier les types et les styles de police, d’insérer des caractères spéciaux et de créer des hyperliens.

L’éditeur de fragment de document peut également insérer des conditions intégrées dans votre texte, comme le montre cette [vidéo](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>Assurez-vous que les éléments de modèle de données de formulaire que vous insérez dans un document sont des descendants de l’élément racine. Par exemple, dans ce cas pratique, assurez-vous que les éléments de l’objet utilisateur que vous sélectionnez sont l’enfant de l’objet soldes .

