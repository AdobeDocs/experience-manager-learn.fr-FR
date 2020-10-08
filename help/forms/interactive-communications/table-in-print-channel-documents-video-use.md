---
title: Utilisation du composant Tableau dans le Document Canal d'impression AEM Forms
seo-title: Utilisation du composant Tableau dans le Document Canal d'impression AEM Forms
description: La vidéo suivante décrit les étapes requises pour utiliser le composant de tableau dans Interactive Communications pour les documents de canal d'impression.
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Utilisation du composant Tableau dans le Document Canal d&#39;impression AEM Forms {#using-table-component-in-aem-forms-print-channel-document}

La vidéo suivante décrit les étapes requises pour utiliser le composant de tableau dans Interactive Communications pour les documents de canal d&#39;impression.

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

Les tableaux sont utilisés pour afficher les données sous forme de tableaux. Les rangées du tableau doivent s’agrandir ou se rétrécir en fonction des données renvoyées par la source de données. Pour utiliser un tableau dans le document de canal d’impression, nous devons créer un fichier de mise en page (fichier xdp) à l’aide de AEM Forms Designer. Dans ce fichier de mise en page, nous ajoutons le tableau avec le nombre requis de colonnes. Assurez-vous que le type d’objet de champ de colonne est TextField ou Numeric Field selon vos besoins. Pour chaque colonne, les champs s’assurent que la liaison de données est définie sur Nom d’utilisation.

>[!NOTE]
>
>Pour rendre le tableau dynamique, assurez-vous d’avoir marqué la rangée comme se répétant.

**Testez-le sur votre propre serveur**

* [Téléchargez et décompressez le fichier de ressources sur votre disque dur.](assets/usingtablesinprintchannel.zip)

* Importer les deux fichiers zip dans AEM à l’aide du gestionnaire de packages

* Les ressources associées à cet article sont les suivantes :

   * Fragment de disposition

   * Modèle de données de formulaire

   * Document de communication interactive
   * sampleretirementaccountdata.json

* Ouvrez le Document de communication interactive en mode [d’](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)édition.

* Ajoutez le fragment de disposition TableDemo dans la section des contributions.
* Lier les cellules du tableau aux éléments appropriés du modèle de données de formulaire, comme le montre la vidéo

* Prévisualisation du Document de communication interactive avec le fichier de données json fourni

