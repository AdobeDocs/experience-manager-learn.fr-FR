---
title: Utiliser les ressources Adobe Stock avec AEM Assets
description: AEM permet aux utilisateurs et aux utilisatrices de rechercher, de prévisualiser, d’enregistrer et d’acquérir des ressources Adobe Stock sous licence directement à partir d’AEM. Les organisations peuvent désormais intégrer leur forfait entreprise Adobe Stock à AEM Assets pour s’assurer que les ressources sous licence sont désormais disponibles pour tous leurs projets de création et marketing, avec les puissantes fonctionnalités de gestion de ressources numériques d’AEM.
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1130
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 100%

---

# Utiliser Adobe Stock avec AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 permet aux utilisateurs et aux utilisatrices de rechercher, de prévisualiser, d’enregistrer et d’acquérir des ressources Adobe Stock sous licence directement à partir d’AEM. Les organisations peuvent désormais intégrer leur forfait entreprise Adobe Stock à AEM Assets pour s’assurer que les ressources sous licence sont désormais disponibles pour tous leurs projets de création et marketing, avec les puissantes fonctionnalités de gestion de ressources numériques d’AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>L’intégration nécessite un [forfait entreprise Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) et AEM 6.4 avec au moins le pack de services 2 déployé. Pour plus d’informations sur le pack de services pour AEM 6.4, consultez ces [notes de mise à jour](https://helpx.adobe.com/fr/experience-manager/6-4/release-notes/sp-release-notes.html).

L’intégration d’Adobe Stock à AEM Assets permet aux personnes qui créent du contenu et aux personnes chargées du marketing d’obtenir facilement des licences et d’utiliser le catalogue de ressources à des fins de création ou de marketing. Pour effectuer une recherche dans le catalogue de ressources, deux options s’offrent à vous : utilisez la recherche omnicanale en ajoutant le filtre d’emplacement sur Adobe Stock ou accédez à la navigation principale d’AEM Assets et cliquez sur l’icône Coral UI Rechercher dans Adobe Stock.

## Fonctionnalités

### Rechercher et enregistrer

* Recherchez des ressources Adobe Stock sans quitter l’espace de travail AEM.
* Enregistrez les ressources Adobe Stock pour prévisualisation, sans licence d’utilisation.
* Possibilité d’obtenir des licences et d’enregistrer des ressources Adobe Stock dans AEM Assets.
* Possibilité de rechercher des ressources Adobe Stock similaires dans l’interface utilisateur d’AEM Assets.
* Recherchez une ressource Adobe Stock dans AEM Assets et affichez-la sur le site web Adobe Stock.
* Les fichiers de ressources sous licence sont marqués d’un badge bleu sous licence pour une identification facile.

### Métadonnées de la ressource

* La ressource sous licence est stockée dans AEM Assets. Les propriétés de ressource contiennent des métadonnées Adobe Stock sous un onglet de métadonnées de ressource distinct.
* Possibilité d’ajouter des références de licence aux métadonnées des ressources.

### Profil Stock des ressources

* Sélectionnez un profil Adobe Stock sous *Utilisateur > Mes préférences > Configuration Stock*.
* Vous pouvez ajouter des références obligatoires et facultatives à la fenêtre Licences des ressources.
* Possibilité de choisir les préférences linguistiques pour la fenêtre Licences des ressources.

### Filtrer

* Un utilisateur ou une utilisatrice peut filtrer les ressources Stock en fonction du type de ressource, de l’orientation et de l’affichage des éléments similaires.
* Le type de ressource comprend des photos, des illustrations, des vecteurs, des vidéos, des modèles, ainsi que des ressources 3D, Premium et éditoriales.
* L’orientation comprend les options suivantes : horizontal, vertical et carré.
* Le filtre Afficher les éléments similaires nécessite un numéro de fichier Adobe Stock.

### Contrôle d’accès

* Les administrateurs et les administratrices peuvent accorder des autorisations à certains utilisateurs et utilisatrices ou groupes pour acquérir des ressources Stock sous licence lors de la configuration du service cloud d’Adobe Stock.
* Si une personne utilisatrice ou un groupe spécifique n’est pas autorisé(e) à acquérir des ressources Stock sous licence, la fonctionnalité *Recherche de ressources/licences de ressources Stock* sera désactivée.

## Configurer Adobe Stock avec AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 permet aux utilisateurs et aux utilisatrices de rechercher, de prévisualiser, d’enregistrer et d’acquérir des ressources Adobe Stock sous licence directement à partir d’AEM. Cette vidéo explique rapidement comment configurer Adobe Stock avec AEM Assets à l’aide de la console Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Pour la configuration du service cloud d’Adobe Stock, vous devez sélectionner l’environnement de production et le chemin d’accès aux ressources sous licence sur `/content/dam`. Le champ Environnement a été supprimé dans AEM.

>[!NOTE]
>
>L’intégration nécessite un [forfait entreprise Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) et AEM 6.4 avec au moins le [pack de services 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) déployé. Pour plus d’informations sur le pack de services AEM 6.4, consultez ces [notes de mise à jour](https://helpx.adobe.com/fr/experience-manager/6-4/release-notes/sp-release-notes.html). Vous avez également besoin d’autorisations d’administration pour la [console Adobe I/O](https://console.adobe.io/), l’[Adobe Admin Console](https://adminconsole.adobe.com/) et Adobe Experience Manager pour configurer l’intégration.

### Installation {#installations}

* Pour AEM 6.4, vous devez installer le [pack de services AEM 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) puis réinstaller le fichier cq-dam-stock-integration-content-1.0.4.zip.
* Assurez-vous que vous disposez des autorisations d’administration pour la [console Adobe I/O](https://console.adobe.io/), l’[Adobe Admin Console](https://adminconsole.adobe.com/) et Adobe Experience Manager pour configurer l’intégration.

#### Configurer Adobe IMS à l’aide de la console Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Créez une configuration de compte technique Adobe IMS sous **Outils > Sécurité**.
2. Pour la *Solution cloud*, sélectionnez *Adobe Stock* et créez un nouveau certificat ou réutilisez un certificat existant pour la configuration.
3. Accédez à la console Adobe I/O et créez une nouvelle intégration de compte de service pour *Adobe Stock*.
4. Chargez le certificat de l’étape 2 vers votre intégration de compte de service Adobe Stock.
5. Sélectionnez la configuration de profil Adobe Stock requise et terminez l’intégration du service.
6. Utilisez les détails de l’intégration pour terminer la configuration du compte technique Adobe IMS.
7. Vérifiez que vous pouvez recevoir le jeton d’accès à l’aide du compte technique Adobe IMS.

![Compte technique Adobe IMS.](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurer les services cloud d’Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Créez une configuration du service cloud pour Adobe Stock sous **Outils > Services Cloud.**
2. Sélectionnez la *Configuration Adobe IMS* créée dans la section ci-dessus pour votre configuration du *cloud Adobe Stock*.

3. Pour **ENVIRONNEMENT**, veillez à sélectionner PROD.
4. Le **Chemin d’accès aux ressources sous licence** peut être pointé vers n’importe quel répertoire sous `/content/dam`.
5. Sélectionnez vos paramètres régionaux et terminez la configuration.
6. Vous pouvez également ajouter des utilisateurs et des utilisatrices ou des groupes à votre service cloud d’Adobe Stock afin d’activer l’accès pour des utilisateurs et des utilisatrices ou des groupes spécifiques.

![Configuration de Stock avec Adobe Assets.](assets/screen_shot_2018-10-22at12425pm.png)

### Ressources supplémentaires

* [Forfait entreprise Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notes de mise à jour du pack de services 2 AEM 6.4](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=fr)
* [Intégration d’AEM et d’Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=fr)
* [API d’intégration de la console Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documents de l’API Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)
