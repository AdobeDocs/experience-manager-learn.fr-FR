---
title: 'Présentation des fichiers d''InDesign et des modèles de ressources dans AEM Assets '
description: Ce didacticiel vidéo décrit la définition d’un fichier d’InDesign et toutes les considérations qui l’accompagnent, en vue de son utilisation dans la fonction Modèles de ressources d’AEM Assets.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# Présentation des fichiers d&#39;InDesign et des modèles de ressources dans AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Ce didacticiel vidéo décrit la définition d’un fichier d’InDesign et toutes les considérations qui l’accompagnent, en vue de son utilisation dans la fonction Modèles de ressources d’AEM Assets.

## Création du fichier de modèle d’InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Téléchargement et ouverture du modèle de fichier d’ [**InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Ouvrez le panneau Balises,** passez en revue la convention d’affectation de nom des balises et notez que les éléments pouvant être créés dans le fichier d’InDesign sont déjà balisés. N’oubliez pas que seuls les éléments balisés sont modifiables dans AEM.

   * **Fenêtre > Utilitaires > Balises**

3. Sur la page, Ajoutez un nouvel élément de texte, fournissez le texte &quot;En-tête&quot; et appliquez le style de paragraphe **d’en-tête** .

   * **Fenêtre > Styles > Styles de paragraphe**

   Ensuite, créez et appliquez une nouvelle balise nommée **Page2Heading.**

4. ajoutez l’image de logo FPO ([fournie dans le fichier zip](assets/asset-templates-tutorial-video--supporting-files.zip)) à l’élément Logo de la page Principal.

   * **Cliquez avec le bouton droit** et sélectionnez **Ajustement > Options d’ajustement des cadres... > Ajustement du contenu > Remplir le bloc proportionnellement.**
   [En savoir plus sur les options](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)d’ajustement des cadres et sur ce qui convient à votre cas d’utilisation.

5. Copiez l’en-tête (logo et nom de la Société) du modèle de Principal dans Page et Page via Coller en place.

   * Sur la page 1, cliquez en maintenant la touche Maj enfoncée sur macOS ou en maintenant la touche Maj enfoncée sur Windows pour sélectionner l’en-tête exposé dans la page du Principal et le supprimer.
   * Depuis la page Principal, copiez l’en-tête dans la page 1 via Coller en place.
   * Répétez les étapes de la page 2

6. Ouvrez le panneau Structure, en cliquant sur chaque doublon, assurez-vous que tous les éléments structuraux correspondent aux éléments réels du fichier d&#39;InDesign. Supprimez les éléments non utilisés ou inutiles. Assurez-vous que tout balisage est sémantique et que les éléments sont correctement balisés.

   >[!NOTE]
   >
   >N’oubliez pas qu’un fichier d’InDesign mal construit est la cause la plus fréquente de problèmes avec les modèles d’actifs AEM. Assurez-vous donc que le balisage et la structure sont propres et corrects.

## Création et création d’un modèle de fichier en AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign Server** début sur le port 8080.
2. Assurez-vous que l’instance d’auteur **AEM est configurée pour interagir avec votre InDesign Server**(et vice versa).

   * [Configuration du Cloud Service de travail IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuration du Cloud Service proxy de cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuration OSGi de l’Externalizer AEM](http://localhost:4502/system/console/configMgr)

3. **Téléchargement du fichier d’InDesign vers AEM Assets** et autorisation à AEM Workflow et à l’InDesign Server de traiter entièrement les ressources.
4. **Créez un nouveau modèle** sous **Ressources > Modèles** et sélectionnez le fichier d’InDesign téléchargé vers AEM à l’étape 4.
5. **Modifiez le modèle** de ressource créé à l’étape 5 et créez les champs modifiables.
6. Cliquez sur **Terminé** pour générer les rendus finaux haute fidélité du modèle de ressource.
7. Cliquez sur la carte Modèle de fichier pour l’ouvrir et passez en revue les rendus de fichier pour télécharger les rendus haute fidélité.

## Ressources supplémentaires {#additional-resources}

Fichier de modèle d’InDesign et images prises en charge

Téléchargement du fichier de modèle d’ [InDesign et des images prises en charge](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Téléchargement d’évaluation InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Téléchargement d’évaluation InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
