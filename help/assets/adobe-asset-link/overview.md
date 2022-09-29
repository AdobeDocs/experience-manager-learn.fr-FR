---
title: Adobe d’un lien et d’une AEM de ressources
description: Les ressources Adobe Experience Manager peuvent être utilisées par les concepteurs et les utilisateurs créatifs dans leurs applications de bureau Adobe Creative Cloud préférées. L’extension Adobe Asset Link pour Adobe Creative Cloud for enterprise étend la fonctionnalité de recherche et de recherche, de tri, de prévisualisation, de chargement de ressources, d’extraction, de modification, d’archivage et d’affichage des métadonnées des ressources d’AEM dans des outils de Creative Cloud tels qu’Adobe XD, Photoshop, InDesign et Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 2%

---


# Adobe Asset Link 3.0

Les ressources Adobe Experience Manager peuvent être utilisées par les concepteurs et les utilisateurs créatifs dans leurs applications de bureau Adobe Creative Cloud préférées.

L’extension Adobe Asset Link pour Adobe Creative Cloud for enterprise étend la fonctionnalité de recherche et de tri, de tri, de prévisualisation, de chargement de ressources, d’extraction, de modification, d’archivage et d’affichage des métadonnées des ressources AEM dans les applications Creative Cloud.

## Fonctionnalités d’Adobe Asset Link

+ Adobe Asset Link s’intègre à AEM Assets et Assets Essentials.
+ Adobe Asset Link configure automatiquement la connexion aux environnements d’AEM basés sur le cloud (AEM Assets as a Cloud Service et Assets Essentials).
+ Adobe Asset Link est une extension qui fonctionne dans les applications Adobe Creative Cloud :

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Authentification automatique à AEM à l’aide de leur Enterprise ID ou de leur Federated ID d’Adobe
+ Parcourir et rechercher des ressources numériques dans AEM
+ Accédez aux détails du fichier pour les ressources résidant dans AEM à partir du panneau :
   + Miniature
   + Métadonnées de base
   + Versions
+ Placer, télécharger ou faire glisser des ressources dans leur mise en page
+ Modifier des ressources en les extrayant des AEM et en les utilisant (en cours) dans leur compte Ressources du Creative Cloud
+ Rearchivez une ressource dans AEM une fois qu’elle a été modifiée, et la nouvelle version est répercutée dans AEM
+ Recherchez des ressources dans AEM à partir du panneau In-App de lien de ressource Adobe
+ Parcourir les collections AEM Assets et les collections dynamiques directement à partir du panneau Asset Link
+ Ajout de ressources nouvellement créées à AEM directement à partir du panneau
+ Glisser-déposer des ressources directement dans des cadres d’InDesign

## Placement de ressources dans InDesign

Adobe Asset Link fournit une prise en charge de liaison directe InDesign entre Adobe Asset Link et AEM. Avec la prise en charge des liens directs InDesign, vous pouvez placer (__Placer les liens__ ou __Placer une copie__) ou faire glisser des ressources numériques vers InDesign à partir d’AEM via le panneau Adobe Asset Link . Présente également le rendu *For Placement Only+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilisez votre Enterprise ID ou votre Federated ID Adobe Creative Cloud uniquement. Assurez-vous que vous [configuration d’AEM pour Adobe Asset Link](https://helpx.adobe.com/fr/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Vous pouvez placer une ressource dans la disposition de votre InDesign à l’aide de l’une des options suivantes :

+ **Placer une copie** - L’incorporation d’une ressource (à l’aide de l’option Placer une copie) place une copie de la ressource d’origine dans votre disposition d’InDesign après le téléchargement des binaires sur votre système local. Adobe Asset Link ne conserve aucun lien entre la copie incorporée et la ressource d’origine. Si la ressource d’origine est modifiée dans AEM, vous devez supprimer la ressource incorporée du fichier d’InDesign et la réincorporer dans AEM.

+ **Placer les liens** - Lorsque vous utilisez des documents InDesign, vous avez la possibilité de référencer les ressources d’AEM en plus de les incorporer directement (à l’aide de l’option Placer une copie du menu contextuel). Le référencement de ressources vous permet de collaborer avec d’autres utilisateurs et d’incorporer toutes les mises à jour apportées à la ressource d’origine dans AEM. Pour référencer une ressource à partir d’AEM, utilisez l’option Placer le lien dans le menu contextuel.

### Images For Placement Only

Lorsque des fichiers de ressources volumineux sont placés dans des documents InDesign à partir d’AEM à l’aide d’Adobe Asset Link, les utilisateurs créatifs doivent attendre quelques secondes après avoir lancé l’opération d’emplacement. Cela a un impact sur l’expérience globale de l’utilisateur. Avec Adobe Asset Link, vous pouvez temporairement placer une image basse résolution de la ressource d’origine à partir de l’AEM, ce qui réduit le temps nécessaire à la place d’une image. En même temps, cela augmente l’expérience globale de l’utilisateur et sa productivité. L’image à résolution inférieure est placée temporairement. Lorsque la sortie finale est requise pour l’impression ou la publication, vous devez remplacer les rendus FPO par les rendus originaux. Si vous souhaitez remplacer plusieurs images FPO par les images d’origine respectives, accédez à **_Windows > Liens_** puis téléchargez les ressources d’origine. Une fois les images d’origine téléchargées, sélectionnez Remplacer tous les FPO par les originaux.

Les rendus FPO sont des substituts légers des ressources d’origine. Elles ont les mêmes proportions, mais sont de taille inférieure à celles des images d’origine. Actuellement, InDesign ne prend en charge l’importation de rendus FPO que pour les types d’image suivants :

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Si un rendu FPO n’est pas disponible pour une ressource spécifique dans AEM, la ressource haute résolution d’origine est référencée à la place. Pour les images FPO, l’état FPO s’affiche dans le panneau Liens d’InDesign .

## Authentification Adobe Asset Link avec AEM Assets

Fonctionnement de l’authentification d’Adobe Asset Link dans le contexte des services Identity Management (IMS) d’Adobe et de l’auteur Adobe Experience Manager.

![Architecture d’Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. L’extension Adobe Asset Link émet une demande d’autorisation, via l’application de bureau Adobe Creative Cloud, pour Adobe Identity Manager Service (IMS) et, en cas de succès, reçoit un jeton porteur.
1. L’extension Adobe Asset Link se connecte à l’auteur AEM via HTTP(S), y compris le jeton porteur obtenu dans **Étape 1**, à l’aide du schéma (HTTP/HTTPS), de l’hôte et du port fournis dans les paramètres JSON de l’extension.
1. Le gestionnaire d’authentification du porteur d’AEM extrait le jeton du porteur de la requête et le valide avec Adobe IMS.
1. Une fois qu’Adobe IMS valide le jeton porteur, un utilisateur est créé dans AEM (s’il n’existe pas déjà) et synchronise les données de profil et de groupe/adhésions d’Adobe IMS. L’utilisateur AEM se voit attribuer un jeton de connexion AEM standard, qui est renvoyé à l’extension Adobe Asset Link en tant que cookie sur la réponse HTTP(S).
1. Interactions suivantes (c.-à-d. navigation, recherche, archivage/extraction de ressources, etc.) avec l’extension Adobe Asset Link, les résultats sont des requêtes HTTP(S) à l’auteur AEM qui sont validées à l’aide du jeton de connexion AEM, à l’aide du gestionnaire d’authentification de jeton AEM standard.

>[!NOTE]
>
>À l’expiration du jeton de connexion, **Étapes 1-5** appelle automatiquement, authentifie l’extension Adobe Asset Link à l’aide du jeton porteur et réémet un nouveau jeton de connexion valide.

## Ressources supplémentaires

+ [Adobe du site Web Asset Link](https://www.adobe.com/fr/creativecloud/business/enterprise/adobe-asset-link.html)
