---
title: Acroforms avec AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Partie 1 de l’intégration d’Acrobat à AEM Forms. Création d’un formulaire adaptatif à l’aide d’Acrobat et fusion des données pour obtenir un PDF.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 3%

---


# Création d’Acrobat

Les Acroforms sont des formulaires créés à l’aide d’Acrobat. Vous pouvez créer un formulaire entièrement nouveau à l’aide d’Acrobat ou prendre un formulaire existant créé dans Microsoft Word et le convertir en Acrobat à l’aide d’Acrobat. Les étapes suivantes doivent être suivies pour convertir un formulaire créé dans Microsoft Word en Acrobat.

* Ouvrir un document Word à l’aide d’Acrobat
* Utilisez l’outil de préparation de formulaire Acrobat pour identifier les champs de formulaire du formulaire.
* Enregistrez le pdf. Assurez-vous que le nom de fichier ne comporte aucun espace.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Si vous souhaitez envoyer l’acroformulaire à remplir pour signature à l’aide d’Acrobat Sign, attribuez un nom aux champs. Par exemple, vous pouvez nommer un champ. **Sig_es_:signer1:signature**. Il s’agit de la syntaxe qu’Acrobat Sign comprend.

>[!NOTE]
>
>Si vous envoyez un document basé sur XFA, vous devez aplatir le document et les balises de signature Acrobat Sign doivent être présentes en tant que texte statique dans le document.

[Document de balises de texte Acrobat Sign](https://helpx.adobe.com/fr/sign/using/text-tag.html)

>[!NOTE]
>
>Assurez-vous que le nom du fichier acroform ne contient aucun espace. L’exemple de code actuel ne prend pas en charge les espaces.
>
>Les noms des champs de formulaire ne peuvent contenir que les éléments suivants :
>
>* espace unique
>* trait de soulignement unique
>* Caractères alphanumériques

