---
title: Acroformes avec AEM Forms
seo-title: Fusionner les données de formulaire adaptatif avec Acroform
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 3%

---


# Création d’Acroform

Les Acroforms sont des formulaires créés à l’aide d’Acrobat. Vous pouvez créer un formulaire à partir de zéro à l’aide d’Acrobat ou prendre un formulaire existant créé dans Microsoft Word et le convertir en Acroform à l’aide d’Acrobat. Les étapes suivantes doivent être suivies pour convertir un formulaire créé dans Microsoft Word en Acroform.

* Document de mot ouvert à l’aide de Acrobat
* Utilisez l’outil de préparation de formulaire Acrobat pour identifier les champs du formulaire.
* Enregistrez le fichier pdf. Assurez-vous que le nom de fichier ne contient aucun espace.


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>Si vous souhaitez envoyer l’acroformulaire à remplir pour signature à l’aide d’Adobe Sign, veuillez nommer les champs en conséquence. Par exemple, vous pouvez nommer un champ **Sig_es_:signataire1:signature**. Voici la syntaxe que Adobe Sign comprend.

>[!NOTE]
>
>Si vous envoyez un document XFA, vous devez aplatir le document et les balises de signature Adobe Sign doivent être présentes sous forme de texte statique dans le document.

[document Balises de texte Adobe Sign](https://helpx.adobe.com/fr/sign/using/text-tag.html)

>[!NOTE]
>
>Assurez-vous que le nom du fichier acroform ne contient aucun espace. L&#39;exemple de code actuel ne gère pas les espaces.
>
>Les noms de champ de formulaire ne peuvent contenir que les éléments suivants :
>
>* espace unique
>* soulignement unique
>* Caractères alphanumériques

