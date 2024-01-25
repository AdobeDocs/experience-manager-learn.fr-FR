---
title: AcroForms avec AEM Forms
description: Partie 1 de l’intégration d’Acroforms à AEM Forms. Création d’un formulaire adaptatif à l’aide d’Acroform et fusion des données pour obtenir un PDF.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 100%

---


# Création d’Acroforms

Les AcroForms sont des formulaires créés à l’aide d’Acrobat. Vous pouvez créer un formulaire entièrement nouveau à l’aide d’Acrobat ou utiliser un formulaire existant créé dans Microsoft Word et le convertir en Acroform à l’aide d’Acrobat. Suivez les étapes suivantes pour convertir un formulaire créé dans Microsoft Word en Acroform.

* Ouvrir un document Word à l’aide d’Acrobat
* Utilisez l’outil de préparation de formulaire Acrobat pour identifier les champs du formulaire.
* Enregistrez le PDF. Assurez-vous que le nom de fichier ne contient aucun espace.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>Si vous souhaitez envoyer l’Acroform à remplir pour signature à l’aide d’Acrobat Sign, nommez les champs en conséquence. Par exemple, vous pouvez nommer un champ **Sig_es_:signer1:signature**. Il s’agit de la syntaxe qu’Acrobat Sign comprend.

>[!NOTE]
>
>Si vous envoyez un document basé sur XFA, vous devez aplatir le document et les balises de signature Acrobat Sign doivent être présentes en tant que texte statique dans le document.

[Document sur les balises de texte Acrobat Sign](https://helpx.adobe.com/fr/sign/using/text-tag.html)

>[!NOTE]
>
>Assurez-vous que le nom de fichier Acroform ne contient aucun espace. L’exemple de code actuel ne prend pas en charge les espaces.
>
>Les noms des champs de formulaire ne peuvent contenir que les éléments suivants :
>
>* espace unique
>* tiret bas unique
>* Caractères alphanumériques
