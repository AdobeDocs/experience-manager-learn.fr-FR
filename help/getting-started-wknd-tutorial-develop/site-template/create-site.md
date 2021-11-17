---
title: Création d’un site | Création AEM site rapide
description: Dans le cadre de la création rapide d’un site, utilisez l’Assistant de création de site dans Adobe Experience Manager, AEM, pour générer un nouveau site web. Le modèle de site standard fourni par Adobe est utilisé comme point de départ pour le nouveau site.
sub-product: sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 2%

---

# Création d’un site {#create-site}

>[!CAUTION]
>
> L’outil de création de site rapide est actuellement un aperçu technique. Il est mis à disposition à des fins de test et d’évaluation et n’est pas destiné à un usage en production sauf accord avec le support Adobe.

Dans le cadre de la création rapide d’un site, utilisez l’Assistant de création de site dans Adobe Experience Manager, AEM, pour générer un nouveau site web. Le modèle de site standard fourni par Adobe est utilisé comme point de départ pour le nouveau site.

## Prérequis {#prerequisites}

Les étapes de ce chapitre se dérouleront dans un environnement Adobe Experience Manager as a Cloud Service. Assurez-vous que vous disposez d’un accès administratif à l’environnement AEM. Il est recommandé d’utiliser une [Programme Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) et [Environnement de développement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=fr) lorsque vous suivez ce tutoriel.

Consultez la section [documentation d’intégration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=fr) pour plus d’informations.

## Objectif {#objective}

1. Découvrez comment utiliser l’ assistant de création de site pour générer un nouveau site.
1. Comprendre le rôle des modèles de site.
1. Explorez le site AEM généré.

## Connexion à Adobe Experience Manager Author {#author}

Pour commencer, connectez-vous à votre environnement as a Cloud Service AEM. Les environnements AEM sont fractionnés en un **Service de création** et un **Service de publication**.

* **Service de création** : emplacement où le contenu du site est créé, géré et mis à jour. En règle générale, seuls les utilisateurs internes ont accès au **Service de création** et se trouve derrière un écran de connexion.
* **Service de publication** - héberge le site web actif. Il s’agit du service que les utilisateurs finaux verront et qui est généralement disponible publiquement.

La majorité du tutoriel se déroule à l’aide de la fonction **Service de création**.

1. Accédez à Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Connectez-vous à l’aide de votre compte personnel ou d’un compte d’entreprise/école.
1. Assurez-vous que la bonne organisation est sélectionnée dans le menu et cliquez sur **Experience Manager**.

   ![Accueil Experience Cloud](assets/create-site/experience-cloud-home-screen.png)

1. Sous **Cloud Manager** click **Launch**.
1. Pointez sur le programme que vous souhaitez utiliser, puis cliquez sur le **Programme Cloud Manager** icône .

   ![Icône du programme Cloud Manager](assets/create-site/cloud-manager-program-icon.png)

1. Dans le menu supérieur, cliquez sur **Environnements** pour afficher les environnements configurés.

1. Recherchez l’environnement que vous souhaitez utiliser, puis cliquez sur le bouton **URL de création**.

   ![Accès à l’auteur de développement](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Il est recommandé d’utiliser une **Développement** environnement pour ce tutoriel.

1. Un nouvel onglet sera lancé sur l’AEM **Service de création**. Cliquez sur **Connexion avec Adobe** et vous devriez être connecté automatiquement avec les mêmes informations d’identification d’Experience Cloud.

1. Après avoir été redirigé et authentifié, vous devriez maintenant voir l’écran de démarrage de l’AEM.

   ![AEM Écran de démarrage](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Vous rencontrez des problèmes pour accéder à Experience Manager ? Consultez la section [documentation d’intégration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Téléchargement du modèle de site de base

Un modèle de site fournit un point de départ pour un nouveau site. Un modèle de site comprend des thèmes de base, des modèles de page, des configurations et des exemples de contenu. Les éléments inclus dans le modèle de site dépendent du développeur. Adobe fournit un **Modèle de site de base** pour accélérer les nouvelles mises en oeuvre.

1. Ouvrez un nouvel onglet du navigateur et accédez au projet Modèle de site de base sur GitHub : [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). Le projet est open-source et sous licence pour être utilisé par n&#39;importe qui.
1. Cliquez sur **Versions** et accédez au [dernière version](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Développez l’objet **Ressources** et téléchargez le fichier zip du modèle :

   ![Zip du modèle de site de base](assets/create-site/template-basic-zip-file.png)

   Ce fichier zip sera utilisé dans l’exercice suivant.

   >[!NOTE]
   >
   > Ce tutoriel est écrit à l’aide de la version **1.1.0** du modèle de site de base. Lors du démarrage d’un nouveau projet pour une utilisation en production, il est toujours recommandé d’utiliser la dernière version.

## Créer un site

Ensuite, générez un nouveau site à l’aide du modèle de site de l’exercice précédent.

1. Revenez à l’environnement AEM. Dans l’écran AEM Démarrer , accédez à **Sites**.
1. Dans le coin supérieur droit, cliquez sur **Créer** > **Site (modèle)**. Cela permettra d’augmenter le **Créer un assistant de site**.
1. Sous **Sélectionner un modèle de site** cliquez sur **Importer** bouton .

   Téléchargez le **.zip** fichier de modèle téléchargé à partir de l’exercice précédent.

1. Sélectionnez la **Modèle de site AEM de base** et cliquez sur **Suivant**.

   ![Sélectionner le modèle de site](assets/create-site/select-site-template.png)

1. Sous **Détails du site** > **Titre du site** enter `WKND Site`.

   Dans une mise en oeuvre réelle, le &quot;site WKND&quot; serait remplacé par le nom de marque de votre société ou organisation. Dans ce tutoriel, nous simulons la création d’un site pour une marque de style de vie fictive &quot;WKND&quot;.

1. Sous **Nom du site** enter `wknd`.

   ![Détails du modèle de site](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Si vous utilisez un environnement d’AEM partagé, ajoutez un identifiant unique à la variable **Nom du site**. Par exemple `wknd-site-johndoe`. Cela permet de s’assurer que plusieurs utilisateurs peuvent suivre le même tutoriel, sans aucune collision.

1. Cliquez sur **Créer** pour générer le site. Cliquez sur **Terminé** dans le **Succès** une fois AEM création du site web terminée.

## Exploration du nouveau site

1. Accédez à la console AEM Sites, le cas échéant.
1. Une nouvelle **Site WKND** a été généré. Elle comprend une structure de site avec une hiérarchie multilingue.
1. Ouvrez le **Anglais** > **Accueil** en sélectionnant la page et en cliquant sur l’icône **Modifier** dans la barre de menus :

   ![Hiérarchie du site WKND](assets/create-site/wknd-site-starter-hierarchy.png)

1. Le contenu de démarrage a déjà été créé et plusieurs composants peuvent être ajoutés à une page. Testez ces composants pour en avoir une idée. Vous apprendrez les principes de base d’un composant dans le chapitre suivant.

   ![Démarrer la page d&#39;accueil](assets/create-site/start-home-page.png)

   *Exemple de contenu fourni par le modèle de site*

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer votre premier site AEM !

### Étapes suivantes {#next-steps}

Utilisez l’éditeur de page d’Adobe Experience Manager AEM pour mettre à jour le contenu du site dans la variable [Création et publication de contenu](author-content-publish.md) chapitre. Découvrez comment les composants atomiques peuvent être configurés pour mettre à jour le contenu. Comprenez la différence entre les environnements d’auteur et de publication AEM et découvrez comment publier des mises à jour sur le site en direct.
