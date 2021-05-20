---
title: 'Présentation des fichiers d’InDesign et des modèles de ressources dans AEM Assets '
description: Ce tutoriel vidéo vous guide tout au long de la définition d’un fichier d’InDesign et de toutes les considérations qui l’accompagnent, en vue de l’utiliser dans la fonction Modèles de ressources d’AEM Assets.
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Business Practitioner
level: Intermediate
source-git-commit: b4fa992abe22e3a546d651e465d6ffc9e415aee2
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---


# Présentation des fichiers d’InDesign et des modèles de ressources dans AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Ce tutoriel vidéo vous guide tout au long de la définition d’un fichier d’InDesign et de toutes les considérations qui l’accompagnent, en vue de l’utiliser dans la fonction Modèles de ressources d’AEM Assets.

## Création du fichier de modèle d’InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Téléchargez et ouvrez le [**modèle de fichier d’InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Ouvrez le panneau Balises,** passez en revue la convention d’affectation des noms de balise et notez que les éléments pouvant être créés dans le fichier d’InDesign sont déjà balisés. Pour rappel, seuls les éléments balisés sont modifiables dans AEM.

   * **Fenêtre > Utilitaires > Balises**

3. Sur la page, ajoutez un nouvel élément de texte, indiquez le texte &quot;En-tête&quot; et appliquez le style de paragraphe **En-tête**.

   * **Fenêtre > Styles > Styles de paragraphe**

   Ensuite, créez et appliquez une nouvelle balise nommée **Page2Heading.**

4. Ajoutez l’image du logo FPO ([fournie dans le fichier zip](assets/asset-templates-tutorial-video--supporting-files.zip)) à l’élément Logo sur la page de Principal.

   * **Cliquez avec le bouton droit** et **sélectionnez Ajustement > Options d’ajustement des cadres... > Ajustement du contenu > Remplir le cadre proportionnellement.**
   [En savoir plus sur les options](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) d’ajustement des images et qui convient à votre cas d’utilisation.

5. Copiez l’en-tête (Logo et Nom de la société) du modèle de Principal dans Page et Page via Coller en place.

   * Sur la page 1, cliquez en maintenant la touche Maj enfoncée sur macOS ou Maj enfoncée tout en cliquant sur Windows pour sélectionner l’en-tête exposé dans la page Principal et le supprimer.
   * Dans la page Principal, copiez l’en-tête dans la page 1 via Coller en place .
   * Répétez les étapes pour la page 2

6. Ouvrez le panneau Structure , en double-cliquant sur chacun d’eux, assurez-vous que tous les éléments structurels correspondent à des éléments réels dans le fichier d’InDesign. Supprimez tout élément non utilisé ou inutile. Assurez-vous que tous les balises sont sémantiques et que les éléments sont correctement balisés.

   >[!NOTE]
   >
   >N’oubliez pas qu’un fichier d’InDesign mal construit est la cause la plus courante des problèmes liés aux modèles de ressources AEM. Assurez-vous donc que le balisage et la structure sont propres et corrects.

## Création et création d’un modèle de ressource dans AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Démarrez le port 8080 d’InDesign** Server.
2. Assurez-vous que l’**instance d’auteur AEM est configurée pour interagir avec votre InDesign Server** (et vice versa).

   * [Configuration du Cloud Service du traitement IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuration du Cloud Service proxy Cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuration OSGi de l’externaliseur d’AEM](http://localhost:4502/system/console/configMgr)

3. **Téléchargement du fichier d’InDesign vers AEM** Assets et permettre à AEM Workflow et à l’InDesign Server de traiter entièrement les ressources.
4. **Créez un nouveau** modèle sous  **Ressources >** Modèles et sélectionnez le fichier d’InDesign téléchargé vers AEM à l’étape #4.
5. **Modifiez le** modèle de ressource créé à l’étape #5 et créez les champs modifiables.
6. Cliquez sur **Terminé** pour générer les rendus haute fidélité finaux du modèle de ressource.
7. Cliquez sur la carte Modèle de ressources pour ouvrir, puis passez en revue les rendus de ressources pour télécharger les rendus haute fidélité.

## Ressources supplémentaires {#additional-resources}

InDesign du fichier de modèle et prise en charge des images

Télécharger [InDesign du fichier de modèle et prise en charge des images](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Téléchargement d’essai InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Les clients CC Enterprise peuvent contacter leur gestionnaire de compte pour demander une licence d’évaluation par InDesign Server.](https://www.adobe.com/products/indesignserver/faq.html)
