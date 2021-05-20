---
title: Utilisation de ressources Adobe Stock avec AEM Assets
description: 'AEM permet aux utilisateurs de rechercher, de prévisualiser, d’enregistrer et d’acquérir sous licence des ressources Adobe Stock directement à partir d’AEM. Les entreprises peuvent désormais intégrer leur formule d’abonnement pour entreprise Adobe Stock à AEM Assets pour s’assurer que les ressources sous licence sont désormais largement disponibles pour leurs projets de création et marketing, avec les puissantes fonctionnalités de gestion de ressources d’AEM. '
feature: Adobe Stock
version: 6.4, 6.5
topic: Gestion de contenu
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 15%

---


# Utilisation d’Adobe Stock avec AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 permet aux utilisateurs de rechercher, prévisualiser, enregistrer et acquérir sous licence des ressources Adobe Stock directement à partir d’AEM. Les entreprises peuvent désormais intégrer leur formule d’abonnement pour entreprise Adobe Stock à AEM Assets pour s’assurer que les ressources sous licence sont désormais largement disponibles pour leurs projets de création et marketing, avec les puissantes fonctionnalités de gestion de ressources d’AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>Pour que l’intégration soit possible, l’[abonnement Adobe Stock entreprise](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) est requis et AEM 6.4 (Service Pack 2 minimum) doit être déployé. Pour plus d’informations sur les formules de service AEM 6.4, consultez ces [notes de mise à jour](https://helpx.adobe.com/fr/experience-manager/6-4/release-notes/sp-release-notes.html).

L’intégration d’Adobe Stock et d’AEM Assets permet aux auteurs de contenu et aux marketeurs d’acquérir sous licence et d’utiliser facilement des ressources de stock à des fins de création ou de marketing. Vous pouvez effectuer une recherche Stock à l’aide de l’option Omni Search, en ajoutant le filtre d’emplacement en tant qu’Adobe Stock ou en naviguant dans la navigation principale d’AEM Assets et en cliquant sur l’icône Rechercher dans l’interface utilisateur Coral d’Adobe Stock.

## Fonctionnalités

### Recherche et enregistrement

* Effectuez une recherche de ressources Adobe Stock sans quitter AEM espace de travail.
* Enregistrez les ressources Adobe Stock à des fins d’aperçu, sans accorder de licence à la ressource.
* Possibilité d’acquérir sous licence et d’enregistrer des ressources Adobe Stock dans AEM Assets
* Possibilité de rechercher des ressources similaires à partir d’Adobe Stock dans l’interface utilisateur d’AEM Assets
* Affichage d’une ressource sélectionnée à partir de la recherche Stock dans AEM Assets sur le site web d’Adobe Stock
* Les fichiers de ressources sous licence sont marqués d’un badge bleu sous licence pour une identification facile.

### Métadonnées de la ressource

* La ressource sous licence est stockée dans AEM Assets. Les propriétés de ressource contiennent des métadonnées Stock sous un onglet de métadonnées de ressource distinct.
* Possibilité d’ajouter des références de licence aux métadonnées de ressource

### Profil de ressources Stock

* Un utilisateur peut sélectionner le profil Adobe Stock sous *Utilisateur > Mes préférences > Configuration des stocks*
* Vous pouvez ajouter des références obligatoires et facultatives à la fenêtre Licences de ressources .
* Possibilité de choisir les préférences linguistiques pour le créneau de licence des ressources en fonction de la région.

### Filtrer

* Un utilisateur peut filtrer les ressources de stock en fonction du type de ressource, de l’orientation et de l’affichage du même type.
* Le type de ressource comprend des photos, des illustrations, des vecteurs, des vidéos, des modèles, des éléments 3D, Premium, des éléments éditoriaux.
* L’orientation comprend les options Horizontal, Vertical et Carré.
* Afficher un filtre similaire nécessite un numéro de fichier Adobe Stock

### Contrôle d’accès

* Les administrateurs peuvent accorder des autorisations à certains utilisateurs/groupes pour acquérir sous licence des ressources de stock lors de la configuration du service cloud Adobe Stock.
* Si un utilisateur/groupe spécifique n’est pas autorisé à acquérir sous licence des ressources de stock, la fonctionnalité *Recherche de ressources/Licences de ressources* est désactivée.

## Configuration d’Adobe Stock avec AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 permet aux utilisateurs de rechercher, prévisualiser, enregistrer et acquérir sous licence des ressources Adobe Stock directement à partir d’AEM. Cette vidéo explique rapidement comment configurer des stocks Adobe avec AEM Assets à l’aide de la console Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Pour la configuration du service Adobe Stock Cloud, vous devez sélectionner le point d’accès à l’environnement PROD et aux ressources sous licence /content/dam. Le champ Environnement sera supprimé dans la prochaine version d’AEM et le chemin d’accès à la ressource sous licence fait partie d’une fonctionnalité à venir et la prise en charge de ce champ sera introduite dans la prochaine version d’AEM.

>[!NOTE]
>
>Pour que l’intégration soit possible, l’[abonnement Adobe Stock entreprise](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) est requis et AEM 6.4 (Service Pack 2 minimum) doit être déployé. [](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/AEM-6.4.2.0-Service-Pack) Pour plus d’informations sur les Service Packs d’AEM 6.4, consultez ces [notes de mise à jour](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Vous avez également besoin d’autorisations d’administrateur pour [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) et Adobe Experience Manager pour configurer l’intégration.

### Installation {#installations}

* Pour AEM 6.4, vous devez installer [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0), puis réinstaller le fichier cq-dam-stock-integration-content-1.0.4.zip .
* Assurez-vous que vous disposez des autorisations d’administrateur sur [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) et Adobe Experience Manager pour configurer l’intégration.

#### Configuration IMS d’Adobe à l’aide de la console Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Créez une configuration de compte technique IMS d’Adobe sous **Outils > Sécurité**
2. Sélectionnez la *solution cloud* *Adobe Stock* et créez un certificat ou réutilisez un certificat existant pour la configuration.
3. Accédez à Adobe I/O Console et créez une nouvelle intégration de compte de service pour *Adobe Stock*.
4. Téléchargez le certificat de l’étape 2 vers votre intégration de compte de service Adobe Stock.
5. Sélectionnez la configuration de profil Adobe Stock requise et terminez l’intégration du service.
6. Utilisez les détails de l’intégration pour terminer la configuration du compte technique IMS Adobe.
7. Vérifiez que vous pouvez recevoir le jeton d’accès à l’aide du compte technique IMS Adobe.

![Compte technique Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configuration des Cloud Services Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Créez une nouvelle configuration de service cloud pour Adobe Stock sous **Outils > Cloud Services.**
2. Sélectionnez la *configuration IMS d’Adobe* créée dans la section ci-dessus pour votre *configuration Adobe Stock Cloud*

3. Veillez à sélectionner **ENVIRONMENT** comme PROD. L’environnement d’évaluation n’est pas pris en charge et sera supprimé dans la prochaine version d’AEM.
4. **Le** chemin d’accès aux ressources sous licence peut être pointé vers n’importe quel répertoire sous /content/dam. La prise en charge des fonctionnalités de ce champ sera ajoutée dans la prochaine version d’AEM
5. Sélectionnez vos paramètres régionaux et effectuez la configuration.
6. Vous pouvez également ajouter des utilisateurs/groupes à votre service Adobe Stock Cloud afin d’activer l’accès pour des utilisateurs ou des groupes spécifiques.

![Configuration d’Adobe Assets Stock](assets/screen_shot_2018-10-22at12425pm.png)

### Ressources supplémentaires

* [Formule Entreprise Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notes de mise à jour d’AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Intégration d’AEM et d’Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API d’intégration d’Adobe I/O Console](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documents de l’API Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)