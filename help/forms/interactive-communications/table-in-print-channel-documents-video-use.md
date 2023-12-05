---
title: Utiliser le composant Tableau dans le document de canal d’impression AEM Forms
description: La vidéo suivante décrit les étapes requises pour utiliser le composant de tableau dans les communications interactives pour les documents de canal d’impression.
feature: Interactive Communication
doc-type: technical video
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 300
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 100%

---

# Utiliser le composant Tableau dans le document de canal d’impression AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

La vidéo suivante décrit les étapes requises pour utiliser le composant de tableau dans les communications interactives pour les documents de canal d’impression.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

Les tableaux sont utilisés pour afficher les données sous forme tabulaire. Les lignes du tableau doivent être agrandies ou réduites en fonction des données renvoyées par la source de données. Pour utiliser un tableau dans le document du canal d’impression, nous devons créer un fichier de disposition (fichier xdp) à l’aide d’AEM Forms Designer. Dans ce fichier de disposition, nous ajoutons le tableau avec le nombre de colonnes requis. Assurez-vous que le type d’objet de champ de colonne est TextField ou NumericField selon vos besoins. Pour chaque colonne, les champs s’assurent que la liaison de données est définie sur Utiliser le nom.

>[!NOTE]
>
>Pour rendre le tableau dynamique, assurez-vous d’avoir marqué la ligne comme répétitive.

**Essayez sur votre propre serveur.**

* [Téléchargez et décompressez le fichier de ressources sur votre disque dur.](assets/usingtablesinprintchannel.zip)

* Importez les deux fichiers zip dans AEM à l’aide du gestionnaire de packages.

* Les ressources associées à cet article sont les suivantes :

   * Fragment de disposition

   * Modèle de données de formulaire

   * Document de communication interactive
   * sampleretirementaccountdata.json

* Ouvrez le document de communication interactive en [mode de modification](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* Ajoutez le fragment de disposition TableDemo à la section des contributions.
* Liez les cellules du tableau aux éléments de modèle de données de formulaire appropriés, comme indiqué dans la vidéo.

* Prévisualiser le document de communication interactive avec l’exemple de fichier de données json fourni
