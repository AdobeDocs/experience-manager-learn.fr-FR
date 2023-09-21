---
title: Créer un site | Création rapide de site d’AEM
description: Découvrez comment utiliser l’assistant de création de site pour générer un nouveau site web. Le modèle de site standard fourni par Adobe est un point de départ pour le nouveau site.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
source-git-commit: 0c6294ac468ad4ead041a68f381c6781a5c29b44
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 91%

---

# Créer un site {#create-site}

Dans le cadre de la création rapide de site, utilisez l’Assistant de création de site dans Adobe Experience Manager (AEM) pour générer un nouveau site web. Le modèle de site standard fourni par Adobe est utilisé comme point de départ pour le nouveau site.

## Prérequis {#prerequisites}

Les étapes de ce chapitre se dérouleront dans un environnement Adobe Experience Manager as a Cloud Service. Assurez-vous que vous disposez d’un accès d’administration à l’environnement AEM. Il est recommandé d’utiliser un [programme Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=fr) et un [environnement de développement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=fr) lorsque vous suivez ce tutoriel.

[Programme de production](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) Les environnements peuvent également être utilisés pour ce tutoriel. Veillez toutefois à ce que les activités de ce tutoriel n’affectent pas le travail effectué sur les environnements cibles, car ce tutoriel déploie le contenu et le code dans l’environnement AEM cible.

La variable [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr) peut être utilisé pour des parties de ce tutoriel. Aspects de ce tutoriel qui dépendent des services cloud, tels que [déploiement de thèmes avec le pipeline front-end de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=fr), ne peut pas être exécuté sur le SDK AEM.

Consultez les [documentation d’intégration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=fr) pour plus d’informations.

## Objectif {#objective}

1. Découvrir comment utiliser l’assistant de création de site pour générer un nouveau site.
1. Découvrir le rôle des modèles de site.
1. Explorer le site AEM généré.

## Se connecter au service de création d’Adobe Experience Manager {#author}

Pour commencer, connectez-vous à votre environnement AEM as a Cloud Service. Les environnements AEM sont divisés en un **Service de création** et un **Service de publication**.

* **Service de création** : emplacement où le contenu du site est créé, géré et mis à jour. En règle générale, seuls les utilisateurs et utilisatrices internes ont accès au **service de création** qui nécessie une authentification.
* **Service de publication** : héberge le site web actif. Il s’agit du service que les utilisateurs et utilisatrices finaux verront et qui est généralement disponible publiquement.

La majorité du tutoriel utilisera le **Service de création**.

1. Accédez à Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Connectez-vous à l’aide de votre compte personnel ou d’un compte d’entreprise ou scolaire.
1. Assurez-vous que la bonne organisation est sélectionnée dans le menu et cliquez sur **Experience Manager**.

   ![Accueil d’Experience Cloud.](assets/create-site/experience-cloud-home-screen.png)

1. Sous **Cloud Manager**, cliquez sur **Démarrer**.
1. Pointez sur le programme que vous souhaitez utiliser, puis cliquez sur l’icône du **programme Cloud Manager**.

   ![Icône du programme Cloud Manager.](assets/create-site/cloud-manager-program-icon.png)

1. Dans le menu supérieur, cliquez sur **Environnements** pour afficher les environnements configurés.

1. Recherchez l’environnement que vous souhaitez utiliser, puis cliquez sur l’**URL de création**.

   ![Accès au développement de création.](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Il est recommandé d’utiliser un environnement de **développement** pour ce tutoriel.

1. Un nouvel onglet est lancé sur le **service de création** d’AEM. Cliquez sur **Sign in with Adobe** et cela devrait vous connecter automatiquement avec les mêmes informations d’identification d’Experience Cloud.

1. Après la redirection et l’authentification, vous devriez maintenant voir l’écran de démarrage d’AEM.

   ![Écran de démarrage d’AEM.](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Vous rencontrez des problèmes pour accéder à Experience Manager ? Consultez la [documentation d’intégration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=fr).

## Télécharger le modèle de site de base

Un modèle de site fournit un point de départ pour un nouveau site. Un modèle de site comprend une base de thèmes, de modèles de page, de configurations et d’exemples de contenu. Les éléments inclus dans le modèle de site dépendent du développeur ou de la développeuse. Adobe fournit un **modèle de site de base** pour accélérer les nouvelles implémentations.

1. Ouvrez un nouvel onglet de navigateur et accédez au projet Modèle de site de base sur GitHub : [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). Le projet est open-source et sous licence pour être utilisé par n’importe qui.
1. Cliquez sur **Versions** et accédez à la [dernière version](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Développez la liste déroulante **Ressources** et téléchargez le fichier zip du modèle :

   ![Fichier zip du modèle de site de base.](assets/create-site/template-basic-zip-file.png)

   Ce fichier zip est utilisé dans l’exercice suivant.

   >[!NOTE]
   >
   > Ce tutoriel est écrit à l’aide de la version **1.1.0** du modèle de site de base. Lors du démarrage d’un nouveau projet pour une utilisation en production, il est toujours recommandé d’utiliser la dernière version.

## Créer un nouveau site

Ensuite, générez un nouveau site à l’aide du modèle de site de l’exercice précédent.

1. Revenez à l’environnement AEM. Depuis l’écran de démarrage AEM, accédez à **Sites**.
1. Dans le coin supérieur droit, cliquez sur **Créer** > **Site (modèle)**. Cela affichera l’**Assistant de création de site**.
1. Sous **Sélectionner un modèle de site**, cliquez sur le bouton **Importer**.

   Téléchargez le fichier **.zip** du modèle téléchargé à partir de l’exercice précédent.

1. Sélectionnez le **modèle de site de base d’AEM** et cliquez sur **Suivant**.

   ![Sélection du modèle de site.](assets/create-site/select-site-template.png)

1. Sous **Détails du site** > **Titre du site**, saisissez `WKND Site`.

   Dans une implémentation réelle, « Site WKND » serait remplacé par le nom de marque de votre entreprise ou organisation. Dans ce tutoriel, nous simulons la création d’un site pour une marque de style de vie fictive, « WKND ».

1. Sous **Nom du site**, saisissez `wknd`.

   ![Détails du modèle de site.](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Si vous utilisez un environnement AEM partagé, ajoutez un identifiant unique au **nom du site**. Par exemple `wknd-site-johndoe`. Cela permet de s’assurer que plusieurs utilisateurs et utilisatrices peuvent suivre le même tutoriel sans aucun conflit.

1. Cliquez sur **Créer** pour générer le site. Cliquez sur **Terminé** dans la boîte de dialogue de **Réussite** une fois qu’AEM a terminé la création du site web.

## Explorer le nouveau site

1. Accédez à la console AEM Sites, si ce n’est pas déjà fait.
1. Une nouveau **site WKND** a été généré. Il comprend une structure de site avec une hiérarchie multilingue.
1. Ouvrez **Anglais** > **Page d’accueil** en sélectionnant la page et en cliquant sur le bouton **Modifier** dans la barre de menus :

   ![Hiérarchie de WKND Site.](assets/create-site/wknd-site-starter-hierarchy.png)

1. Le contenu de démarrage a déjà été créé et plusieurs composants peuvent être ajoutés à une page. Testez ces composants pour vous faire une idée de leur fonctionnalité. Vous apprendrez les principes de base d’un composant dans le chapitre suivant.

   ![Démarrage de la page d’accueil.](assets/create-site/start-home-page.png)

   *Exemple de contenu fourni par le modèle de site*

## Félicitations. {#congratulations}

Félicitations, vous venez de créer votre premier site AEM.

### Étapes suivantes {#next-steps}

Utilisez l’éditeur de page d’Adobe Experience Manager (AEM) pour mettre à jour le contenu du site dans le chapitre [Créer et pubier du contenu](author-content-publish.md). Découvrez comment les composants atomiques peuvent être configurés pour mettre à jour le contenu. Comprenez la différence entre les environnements de création et de publication d’AEM et découvrez comment publier des mises à jour sur le site en direct.
