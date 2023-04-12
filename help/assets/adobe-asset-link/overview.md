---
title: Adobe Asset Link et AEM
description: Les ressources d’Adobe Experience Manager peuvent être utilisées par les concepteurs et conceptrices ainsi que les utilisateurs et utilisatrices créatifs dans leurs applications de bureau Adobe Creative Cloud préférées. L’extension Adobe Asset Link pour Adobe Creative Cloud for enterprise étend la fonctionnalité de recherche, de tri, de prévisualisation, de chargement de ressources, d’extraction, de modification, d’archivage et d’affichage des métadonnées des ressources d’AEM dans des outils de Creative Cloud tels qu’Adobe XD, Photoshop, InDesign et Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 100%

---


# Adobe Asset Link 3.0

Les ressources d’Adobe Experience Manager peuvent être utilisées par les concepteurs et conceptrices ainsi que les et utilisateurs et utilisatrices créatifs dans leurs applications de bureau Adobe Creative Cloud préférées.

L’extension Adobe Asset Link pour Adobe Creative Cloud for enterprise étend la fonctionnalité de recherche, de tri, de prévisualisation, de chargement de ressources, d’extraction, de modification, d’archivage et d’affichage des métadonnées des ressources AEM dans les applications Creative Cloud.

## Fonctionnalités d’Adobe Asset Link

+ Adobe Asset Link s’intègre à AEM Assets et Assets Essentials.
+ Adobe Asset Link configure automatiquement la connexion aux environnements d’AEM basés sur le cloud (AEM Assets as a Cloud Service et Assets Essentials).
+ Adobe Asset Link est une extension qui fonctionne dans les applications Adobe Creative Cloud :

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ L’authentification automatique à AEM à l’aide de leur Enterprise ID ou de leur Federated ID d’Adobe.
+ Parcourez et recherchez des ressources numériques dans AEM.
+ Accédez aux détails de fichier pour les ressources résidant dans AEM à partir du panneau :
   + Miniature
   + Métadonnées de base
   + Versions
+ Placez, téléchargez ou faites glisser et déposez des ressources dans leur disposition.
+ Modifiez des ressources en les extrayant d’AEM et en travaillant dessus dans leur compte de ressources Creative Cloud.
+ Rearchivez une ressource dans AEM une fois qu’elle a été modifiée et disposez de la nouvelle version dans AEM.
+ Recherchez des ressources dans AEM à partir du panneau in-app dans Adobe Asset Link.
+ Parcourez les collections d’AEM Assets et les collections dynamiques directement à partir du panneau d’Asset Link.
+ Ajoutez des ressources nouvellement créées à AEM directement à partir du panneau.
+ Faites glisser et déposez des ressources directement dans des blocs InDesign.

## Placer des ressources dans InDesign

Adobe Asset Link fournit une liaison directe avec InDesign entre Adobe Asset Link et AEM. Avec la prise en charge des liens directs à InDesign, vous pouvez placer (__Placer un lien__ ou __Placer une copie__) ou faire glisser des ressources numériques vers InDesign à partir d’AEM via le panneau Adobe Asset Link. Le service offre également le rendu *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Utilisez uniquement votre Enterprise ID ou votre Federated ID d’Adobe Creative Cloud. Assurez-vous de [configurer AEM pour Adobe Asset Link](https://helpx.adobe.com/fr/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Vous pouvez placer une ressource dans votre disposition InDesign à l’aide de l’une des options suivantes :

+ **Placer une copie** : l’incorporation d’une ressource (à l’aide de l’option Placer une copie) place une copie de la ressource d’origine dans votre disposition InDesign après le téléchargement des fichiers binaires sur votre système local. Adobe Asset Link ne conserve aucun lien entre la copie incorporée et la ressource d’origine. Si la ressource d’origine est modifiée dans AEM, vous devez supprimer la ressource incorporée du fichier InDesign et la réincorporer depuis AEM.

+ **Placer un lien** : lorsque vous utilisez des documents InDesign, vous avez la possibilité de référencer les ressources d’AEM en plus de les incorporer directement (à l’aide de l’option Placer une copie du menu contextuel). Le référencement de ressources vous permet de collaborer avec d’autres utilisateurs et utilisatrices, ainsi que d’incorporer toutes les mises à jour apportées à la ressource d’origine dans AEM. Pour référencer une ressource à partir d’AEM, utilisez l’option Placer un lien dans le menu contextuel.

### Images For Placement Only

Lorsque des fichiers de ressources volumineux sont placés dans des documents InDesign à partir d’AEM à l’aide d’Adobe Asset Link, les utilisateurs et utilisatrices créatifs doivent attendre quelques secondes après avoir lancé l’opération de placement. Cela a un impact sur l’expérience globale d’utilisation. Avec Adobe Asset Link, vous pouvez temporairement placer une image basse résolution de la ressource d’origine d’AEM, et réduire ainsi le temps nécessaire au placement de l’image. En même temps, cela améliore globalement l’expérience d’utilisation et la productivité. L’image à résolution inférieure est placée temporairement et lorsque la sortie finale est requise pour l’impression ou la publication, vous devez remplacer les rendus de placement uniquement (FPO, For Placement Only) par les rendus originaux. Si vous souhaitez remplacer plusieurs images FPO par les images d’origine respectives, accédez au panneau **_Windows > Liens_** puis téléchargez les ressources d’origine. Une fois les images d’origine téléchargées, sélectionnez Remplacer tous les FPO par les originaux.

Les rendus FPO sont des substituts légers des ressources d’origine. Ils ont le même format, mais sont de taille inférieure à ceux des images d’origine. Actuellement, InDesign ne prend en charge l’importation de rendus FPO que pour les types d’image suivants :

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Si un rendu FPO n’est pas disponible pour une ressource spécifique dans AEM, la ressource haute résolution d’origine est référencée à la place. Pour les images FPO, le statut FPO s’affiche dans le panneau Liens d’InDesign.

## Authentification Adobe Asset Link avec AEM Assets

Découvrez comment fonctionne l’authentification d’Adobe Asset Link avec les services Identity Management (IMS) d’Adobe et l’environnement de création d’Adobe Experience Manager.

![Architecture Adobe Asset Link.](assets/adobe-asset-link-article-understand.png)

1. L’extension Adobe Asset Link émet une requête d’autorisation via l’application de bureau Adobe Creative Cloud au service Identity Manager (IMS) d’Adobe et, en cas de succès, reçoit un jeton porteur.
1. L’extension Adobe Asset Link se connecte au service de création d’AEM via HTTP(S), en incluant le jeton porteur obtenu à l’**Étape 1**, à l’aide du schéma (HTTP/HTTPS), de l’hôte et du port fournis dans les paramètres JSON de l’extension.
1. Le gestionnaire d’authentification du porteur d’AEM extrait le jeton porteur de la requête et le valide avec Adobe IMS.
1. Une fois qu’Adobe IMS valide le jeton porteur, un utilisateur est créé dans AEM (s’il n’existe pas déjà) et synchronise les données de profil, de groupe et d’adhésions d’Adobe IMS. L’utilisateur ou l’utilisatrice d’AEM se voit attribuer un jeton de connexion AEM standard, qui est renvoyé à l’extension Adobe Asset Link en tant que cookie sur la réponse HTTP(S).
1. Les interactions suivantes (c’est-à-dire la navigation, la recherche, l’archivage et l’extraction de ressources, etc.) avec l’extension Adobe Asset Link génèrent des requêtes HTTP(S) au service de création d’AEM qui sont validées à l’aide du jeton de connexion AEM, avec le gestionnaire d’authentification de jeton AEM standard.

>[!NOTE]
>
>À l’expiration du jeton de connexion, les **Étapes 1 à 5** sont automatiquement appelées et authentifient l’extension Adobe Asset Link à l’aide du jeton porteur. Un nouveau jeton de connexion valide est réémis.

## Ressources supplémentaires

+ [Site web Adobe Asset Link](https://www.adobe.com/fr/creativecloud/business/enterprise/adobe-asset-link.html)
