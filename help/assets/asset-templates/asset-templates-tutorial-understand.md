---
title: Comprendre les fichiers d’InDesign et des modèles de ressources dans AEM Assets
description: Ce tutoriel vidéo vous guide tout au long de la définition d’un fichier d’InDesign et de toutes les considérations qui l’accompagnent, en vue de l’utiliser dans la fonction des modèles de ressources d’AEM Assets.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 925
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 100%

---

# Comprendre les fichiers d’InDesign et des modèles de ressources dans AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Ce tutoriel vidéo vous guide tout au long de la définition d’un fichier d’InDesign et de toutes les considérations qui l’accompagnent, en vue de l’utiliser dans la fonction des modèles de ressources d’AEM Assets.

## Créer le fichier de modèle InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Téléchargez et ouvrez le [**modèle de fichier d’InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip).
2. **Ouvrez le panneau Balises,** passez en revue la convention de nommage des balises et notez que les éléments pouvant être créés dans le fichier InDesign sont déjà balisés. N’oubliez pas que seuls les éléments balisés sont modifiables dans AEM.

   * **Fenêtre > Utilitaires > Balises**

3. Sur la page, ajoutez un nouvel élément de texte, indiquez le texte « En-tête » et appliquez le style de paragraphe **En-tête**.

   * **Fenêtre > Styles > Styles de paragraphe**

   Ensuite, créez et appliquez une nouvelle balise nommée **Page2Heading**.

4. Ajoutez l’image du logo FPO ([fourni dans le fichier zip](assets/asset-templates-tutorial-video--supporting-files.zip)) à l’élément Logo sur le gabarit de page.

   * **Cliquez avec le bouton droit** et sélectionnez **Ajuster > Options d’ajustement des cadres... > Ajustement du contenu > Remplir le cadre proportionnellement**.

   [En savoir plus sur les options d’ajustement des cadres](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), et sur celles qui conviennent le mieux à votre cas d’utilisation.

5. Copiez l’en-tête (Logo et Nom de l’entreprise) du Modèle de gabarit dans la page via Coller en place.

   * Sur la page 1, cliquez en maintenant les touches Maj et Cmd enfoncées sur macOS ou en maintenant les touches Maj et Alt enfoncées sur Windows pour sélectionner l’en-tête exposé dans le gabarit de page et le supprimer.
   * Depuis le gabarit de page, copiez l’en-tête dans la page 1 via Coller en place.
   * Répétez les étapes pour la page 2.

6. Ouvrez le panneau Structure et double-cliquez sur chaque élément structurel pour vérifier qu’ils correspondent à des éléments réels dans le fichier InDesign. Supprimez les éléments non utilisés ou dont vous n’avez pas besoin. Assurez-vous que tous les balises sont sémantiques et que les éléments sont correctement balisés.

   >[!NOTE]
   >
   >N’oubliez pas qu’un fichier InDesign mal créé est la cause la plus courante des problèmes liés aux modèles de ressources AEM. Assurez-vous donc que le balisage et la structure sont propres et corrects.

## Créer un modèle de ressource dans AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Démarrez InDesign Server** sur le port 8080.
2. Assurez-vous que l’**instance de création AEM est configurée pour interagir avec votre InDesign Server** (et vice versa).

   * [Configurer le service cloud du programme de travail IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configurer le service cloud de proxy cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuration OSGi de l’Externalizer AEM](http://localhost:4502/system/console/configMgr)

3. **Chargez le fichier InDesign dans AEM Assets** et autorisez le workflow AEM et InDesign Server à traiter entièrement les ressources.
4. **Créez un modèle** sous **Ressources > Modèles** et sélectionnez le fichier InDesign chargé dans AEM à l’étape 4.
5. **Modifiez le modèle de ressource** créé à l’étape 5 et créez les champs modifiables.
6. Cliquez sur **Terminé** pour générer les rendus haute-fidélité finaux du modèle de ressource.
7. Cliquez sur la carte Modèle de ressources pour l’ouvrir, puis examinez les rendus de ressources pour télécharger les rendus haute-fidélité.

## Ressources supplémentaires {#additional-resources}

Fichier de modèle InDesign et images prises en charge

Téléchargez le [fichier de modèle InDesign et les images prises en charge](assets/asset-templates-tutorial-video--supporting-files-1.zip).

* [Télécharger la version d’essai InDesign CC](https://creative.adobe.com/products/download/indesign)
* La version d’essai InDesign Server peut être téléchargée à partir du [site Adobe Prerelease](https://www.adobeprerelease.com/) ou [les clientes et clients CC Enterprise peuvent contacter la personne chargée de la gestion de leur compte pour demander une licence d’essai InDesign Server.](https://www.adobe.com/fr/products/indesignserver/faq.html)
