---
title: Utilisation de l’extension Adobe Asset Link avec AEM Assets
description: 'Les ressources Adobe Experience Manager peuvent désormais être utilisées par les concepteurs et les utilisateurs créatifs dans leurs applications de bureau Adobe Creative Cloud préférées. L’extension Adobe Asset Link pour Adobe Creative Cloud Enterprise permet de rechercher et de parcourir, de trier, de prévisualiser, de charger des ressources, d’extraire, de modifier, d’archiver et d’afficher les métadonnées des ressources AEM dans des outils de Creative Cloud tels qu’Adobe Photoshop, InDesign et Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: Gestion de contenu
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 2%

---


# Utilisation de l’extension Adobe Asset Link avec AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Les ressources Adobe Experience Manager peuvent désormais être utilisées par les concepteurs et les utilisateurs créatifs dans leurs applications de bureau Adobe Creative Cloud préférées. L’extension Adobe Asset Link pour Adobe Creative Cloud Enterprise permet de rechercher et de parcourir, de trier, de prévisualiser, de charger des ressources, d’extraire, de modifier, d’archiver et d’afficher les métadonnées des ressources AEM dans des outils de Creative Cloud tels qu’Adobe Photoshop, InDesign et Illustrator.


## Adobe Asset Link 1.1

Adobe Asset Link v1.1 fournit désormais la prise en charge de la liaison directe par InDesign entre Adobe Asset Link et AEM Assets. Grâce à la prise en charge des liens directs d’InDesign, vous pouvez désormais placer (Placer les ressources liées ou Placer une copie) ou faire glisser des ressources numériques dans InDesign à partir d’AEM Assets via le panneau Adobe Asset Link . Présente également le rendu *For Placement Only* (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilisez votre Enterprise ID ou votre Federated ID Adobe Creative Cloud uniquement. Veillez à [configurer AEM pour Adobe Asset Link](https://helpx.adobe.com/fr/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).


### Adobe des fonctionnalités Asset Link

* Adobe Asset Link est une extension qui fonctionne avec PS, AI, ID et qui fournit un accès direct aux ressources numériques qui résident dans AEM Assets.
* Les créatifs seront automatiquement connectés à AEM à l’aide de leur Enterprise ID ou Federated ID IMS Adobe
* Les créatifs peuvent parcourir les ressources numériques qui résident dans AEM Assets en plus de rechercher sur AEM Assets et les ressources Creative Cloud.
* Les créatifs peuvent accéder aux détails du fichier pour les ressources résidant dans AEM Assets ; miniature, métadonnées de base et versions du panneau
* Les créatifs peuvent placer, télécharger ou faire glisser des ressources dans leur mise en page.
* Les créatifs peuvent modifier des ressources en les extraire d’AEM Assets et en y travaillant (en cours) dans leur compte Ressources du Creative Cloud.
* Les créatifs peuvent réarchiver une ressource dans AEM Assets une fois qu’ils ont fini de la modifier, et la nouvelle version sera répercutée dans AEM Assets.
* Prend en charge les applications de bureau Creative Cloud 2020, 2019 et 2018, Photoshop et Illustrator
* Un utilisateur peut effectuer une recherche de ressources à partir du panneau In-App de lien de ressource Adobe et les trier selon la taille, l’ordre alphabétique et par pertinence.
* Les utilisateurs peuvent accéder aux collections AEM Assets et aux collections dynamiques et les parcourir directement à partir du panneau Asset Link .
* Ajout de ressources nouvellement créées à AEM Assets directement à partir du panneau
* Un utilisateur peut faire glisser et déposer des ressources directement dans des cadres d’InDesign.

### Placement d’AEM Assets dans un InDesign

Vous pouvez placer une ressource dans la disposition de votre InDesign à l’aide de l’une des options suivantes :

* **Placer une copie**  : l’incorporation d’une ressource (à l’aide de l’option Placer une copie) place une copie de la ressource d’origine dans la disposition de votre InDesign après le téléchargement des binaires sur votre système local. Adobe Asset Link ne conserve aucun lien entre la copie incorporée et la ressource d’origine. Si la ressource d’origine est modifiée dans AEM Assets, vous devez supprimer la ressource incorporée du fichier d’InDesign et la réincorporer à partir d’AEM Assets.

* **Placer les ressources liées**  : lorsque vous utilisez des documents InDesign, vous avez désormais la possibilité de référencer les ressources d’AEM Assets en plus de les incorporer directement (à l’aide de l’option Placer une copie du menu contextuel). Le référencement de ressources vous permet de collaborer avec d’autres utilisateurs et d’incorporer toutes les mises à jour apportées à la ressource d’origine dans AEM Assets. Pour référencer une ressource à partir d’AEM Assets, utilisez l’option Placer le lien dans le menu contextuel.

### Pour la résolution d’emplacement uniquement (FPO)

Lorsque des fichiers de ressources volumineux sont placés dans des documents InDesign à partir d’AEM Assets à l’aide d’Adobe Asset Link, les utilisateurs créatifs doivent attendre quelques secondes après avoir lancé l’opération d’emplacement. Cela a un impact sur l’expérience globale de l’utilisateur. Avec Adobe Asset Link, vous pouvez désormais placer temporairement une image basse résolution de la ressource d’origine à partir d’AEM Assets, ce qui réduit le temps nécessaire pour placer une image. En même temps, cela augmente l’expérience globale de l’utilisateur et sa productivité. L’image à résolution inférieure est placée temporairement. Lorsque la sortie finale est requise pour l’impression ou la publication, vous devez remplacer les rendus FPO par les rendus originaux. Si vous souhaitez remplacer plusieurs images FPO par les images d’origine respectives, accédez au panneau **_Windows > Liens_** , puis téléchargez les ressources d’origine. Une fois les images d’origine téléchargées, sélectionnez Remplacer tous les FPO par les originaux.

>[!NOTE]
>
> *Le rendu For Placement Only (FPO)*  fonctionne uniquement pour l’option Place Linked (Placer Linked). Vous devez également activer la prise en charge du rendu FPO dans le workflow Ressource de mise à jour de gestion des actifs numériques *AEM Assets.*

Les rendus FPO sont des substituts légers des ressources d’origine. Elles ont les mêmes proportions, mais sont de taille inférieure à celles des images d’origine. Actuellement, InDesign ne prend en charge l’importation de rendus FPO que pour les types d’image suivants :

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Si un rendu FPO n’est pas disponible pour une ressource spécifique dans AEM Assets, la ressource haute résolution d’origine est référencée à la place. Pour les images FPO, l’état FPO s’affiche dans le panneau Liens d’InDesign .

## Présentation de l’authentification Adobe Asset Link avec AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Fonctionnement de l’authentification d’Adobe Asset Link dans le contexte des services Identity Management (IMS) d’Adobe et de l’auteur Adobe Experience Manager.

![Architecture d’Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

Télécharger [Adobe Asset Link Architecture](assets/adobe-asset-link-article-understand-1.png)

1. L’extension Adobe Asset Link émet une demande d’autorisation, via l’application de bureau Adobe Creative Cloud, pour Adobe Identity Manager Service (IMS) et, en cas de succès, reçoit un jeton porteur.
2. L’extension Adobe Asset Link se connecte à l’auteur AEM via HTTP(S), y compris le jeton porteur obtenu à l’étape **1**, à l’aide du modèle (HTTP/HTTPS), de l’hôte et du port fournis dans les paramètres JSON de l’extension.
3. Le gestionnaire d’authentification du porteur d’AEM extrait le jeton du porteur de la requête et le valide par rapport à l’Adobe IMS.
4. Une fois qu’Adobe IMS valide le jeton porteur, un utilisateur est créé dans AEM (s’il n’existe pas déjà) et synchronise les données de profil et de groupe/adhésions à partir d’Adobe IMS. L’utilisateur AEM se voit attribuer un jeton de connexion AEM standard, qui est renvoyé à l’extension Adobe Asset Link en tant que cookie sur la réponse HTTP(S).
5. Interactions suivantes (c.-à-d. navigation, recherche, archivage/extraction de ressources, etc.) avec l’extension Adobe Asset Link, les résultats sont des requêtes HTTP(S) à l’auteur AEM qui sont validées à l’aide du jeton de connexion AEM, à l’aide du gestionnaire d’authentification de jeton AEM standard.

>[!NOTE]
>
>À l’expiration du jeton de connexion, **les étapes 1-5** s’affichent automatiquement, authentifiant l’extension Adobe Asset Link à l’aide du jeton porteur et réémet un nouveau jeton de connexion valide.

## Ressources supplémentaires{#additional-resources}

* [Adobe du site Web Asset Link](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)