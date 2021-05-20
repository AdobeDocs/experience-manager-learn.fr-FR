---
title: Utilisation du composant Tableau dans le document Canal d’impression AEM Forms
seo-title: Utilisation du composant Tableau dans le document Canal d’impression AEM Forms
description: La vidéo suivante décrit les étapes requises pour utiliser le composant de tableau dans les communications interactives pour les documents de canal d’impression.
feature: Communication interactive
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 1%

---


# Utilisation du composant Tableau dans le document Canal d’impression AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

La vidéo suivante décrit les étapes requises pour utiliser le composant de tableau dans les communications interactives pour les documents de canal d’impression.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Les tableaux sont utilisés pour afficher les données sous forme tabulaire. Les lignes du tableau doivent s’agrandir ou se rétrécir en fonction des données renvoyées par la source de données. Pour utiliser un tableau dans le document du canal d’impression, nous devons créer un fichier de mise en page (fichier xdp) à l’aide d’AEM Forms Designer. Dans ce fichier de mise en page, nous ajoutons le tableau avec le nombre de colonnes requis. Assurez-vous que le type d’objet de champ de colonne est TextField ou Numeric Field selon vos besoins. Pour chaque colonne, les champs s’assurent que la liaison de données est définie sur Utiliser le nom.

>[!NOTE]
>
>Pour rendre le tableau dynamique, assurez-vous d’avoir marqué la ligne comme répétitive.

**Essayez-le sur votre propre serveur**

* [Téléchargez et décompressez le fichier de ressources sur votre disque dur.](assets/usingtablesinprintchannel.zip)

* Importez les deux fichiers zip dans AEM à l’aide du gestionnaire de package.

* Les ressources associées à cet article sont les suivantes :

   * Fragment de disposition

   * Modèle de données de formulaire

   * Document de communication interactive
   * sampleretirementaccountdata.json

* Ouvrez le document de communication interactive en [mode d’édition](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Ajoutez le fragment de disposition TableDemo à la section des contributions .
* Lier les cellules de tableau aux éléments de modèle de données de formulaire appropriés, comme indiqué dans la vidéo

* Aperçu du document de communication interactive avec l’exemple de fichier de données json fourni

