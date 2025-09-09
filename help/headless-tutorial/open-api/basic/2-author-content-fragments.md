---
title: Créer des fragments de contenu | OpenAPI AEM Headless Partie 2
description: Prise en main de Adobe Experience Manager (AEM) et OpenAPI. Créez et modifiez un fragment de contenu basé sur un modèle de fragment de contenu. Découvrez comment créer des variations de fragments de contenu.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 700
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '1274'
ht-degree: 11%

---

# Créer des fragments de contenu

Dans ce chapitre, vous allez apprendre à créer et modifier des fragments de contenu basés sur les modèles de fragment de contenu [ Équipe et Personne ](./1-content-fragment-models.md). Ces fragments de contenu seront le contenu consommé par l’application React à l’aide de la diffusion de fragments de contenu AEM avec les API OpenAPI.

## Conditions préalables

Avant d’aborder ce tutoriel en plusieurs parties, assurez-vous d’avoir suivi les étapes décrites dans la section [Définir des modèles de fragment de contenu](./1-content-fragment-models.md).

## Objectifs

* Créez un fragment de contenu basé sur un modèle de fragment de contenu.
* Créez un fragment de contenu.
* Publiez un fragment de contenu.

## Créer des dossiers de ressources pour les fragments de contenu

Les fragments de contenu sont stockés dans des dossiers AEM Assets. Pour créer des fragments de contenu à partir des modèles de fragment de contenu créés dans le chapitre précédent, un dossier doit exister pour les stocker. Une configuration du dossier est requise pour permettre la création de fragments de contenu à partir de modèles de fragment de contenu spécifiques.

AEM prend en charge l’organisation des dossiers « plats », ce qui signifie que les fragments de contenu de différents modèles de fragment de contenu sont regroupés dans un seul dossier. Cependant, dans ce tutoriel, une structure de dossiers qui s’aligne sur les modèles de fragment de contenu est utilisée, en partie, pour explorer l’API **Liste de tous les fragments de contenu par dossier** dans le [chapitre suivant](./3-explore-openapis.md). Lorsque vous déterminez votre organisation de fragments de contenu, tenez compte à la fois de la manière dont vous souhaitez créer et gérer vos fragments de contenu, ainsi que de la manière de les diffuser et de les utiliser via la diffusion de fragments de contenu AEM avec les API OpenAPI.

1. Sur l’écran de démarrage d’AEM, accédez à **Ressources** > **Fichiers**.
1. Sélectionnez **Créer** dans le coin supérieur droit, puis sélectionnez **Dossier**. Enter :

   * Titre : **Mon projet**
   * Nom : **mon-projet**

   Pour créer le dossier, sélectionnez **Créer**.

1. Ouvrez le nouveau dossier **Mon projet** et créez un sous-dossier sous le nouveau dossier **Mon projet** avec les valeurs suivantes :

   * Titre : **anglais**
   * Nom : **fr**

   Un dossier de langue racine est créé pour positionner le projet afin de prendre en charge les fonctionnalités de localisation natives d’AEM. Une bonne pratique consiste à configurer des projets pour une prise en charge multilingue, même si vous n’avez pas besoin de localisation aujourd’hui. Consultez [la documentation suivante pour en savoir plus](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=fr).

1. Créez deux sous-dossiers sous le nouveau dossier **Mon projet > Français** avec les valeurs suivantes :

   Un dossier `teams` qui contient les fragments de contenu **Équipe**

   * Titre : **Teams**
   * Nom : **équipes**

   ... et un dossier de `people` contenant les fragments de contenu **Personne**.

   * Titre : **People**
   * Nom : **people**

1. Revenez au dossier **Mon projet > Français** et assurez-vous que les deux nouveaux dossiers sont créés.
1. Sélectionnez le dossier **Équipes** et sélectionnez **Propriétés** dans la barre d’actions supérieure.
1. Sélectionnez l’onglet **Politiques** et décochez la case **Hérité de`/content/dam/my-project`**.
1. Dans l’onglet **Politiques**, sélectionnez le modèle de fragment de contenu **Équipe** dans le champ **Modèles de fragment de contenu autorisés par chemin d’accès**.

   ![Modèles de fragment de contenu autorisés.](assets/2/folder-policies.png)

   Ces politiques sont automatiquement héritées par les sous-dossiers, mais peuvent être remplacées. Les modèles de fragment de contenu peuvent être autorisés par des balises ou activer les modèles de fragment de contenu à partir d’autres configurations de projet. Ce mécanisme permet de gérer efficacement votre hiérarchie de contenu.

1. Sélectionnez **Enregistrer et fermer** pour enregistrer les modifications apportées aux propriétés du dossier.
1. Procédez de la même manière pour mettre à jour les **Politiques** du dossier **Personnes**, mais sélectionnez plutôt le modèle de fragment de contenu **Personne**.

## Créer un fragment de contenu de personne

Créez des fragments de contenu basés sur le modèle de fragment de contenu **Personne** dans le dossier **Mon projet > Anglais > Personnes**.

1. Sur l’écran de démarrage d’AEM, sélectionnez **Fragments de contenu** pour ouvrir la console Fragments de contenu.
1. Sélectionnez le bouton **Afficher le dossier** pour ouvrir l’explorateur de dossiers.
1. Sélectionnez le dossier **Mon projet > Anglais > Personnes**.
1. Sélectionnez **Créer > Fragment de contenu** et saisissez les valeurs suivantes :

   * Emplacement : `/content/dam/my-project/en/people`.
   * Modèle de fragment de contenu : **Personne**
   * Titre : **John Doe**
   * Nom : `john-doe`.

   Gardez à l’esprit que ces champs **Titre**, **Nom** et **Description** de la boîte de dialogue **Nouveau fragment de contenu** sont stockés en tant que métadonnées sur le fragment de contenu et ne font pas partie des données du fragment de contenu.

   ![Nouveau fragment du contenu.](assets/2/new-content-fragment.png)

1. Sélectionnez **Créer et ouvrir**.
1. Renseignez les champs du fragment **John Doe** :

   * Nom Complet : **John Doe**
   * Biographie : **John Doe aime les médias sociaux et les passionnés de voyages.**
   * Photo de profil : sélectionnez une image dans `/content/dam` ou chargez-en une nouvelle.
   * Profession : **influenceur**, **voyageur**

   Ces champs et valeurs définissent le contenu du fragment de contenu qui sera utilisé via la diffusion de fragments de contenu AEM avec les API OpenAPI.

   ![Création d’un fragment de contenu](assets/2/john-doe-content-fragment.png)

1. Les modifications apportées aux fragments de contenu sont automatiquement enregistrées. Il n’y a donc pas de bouton **Enregistrer**.
1. Revenez à la console Fragments de contenu et sélectionnez **Mon projet > Français > Personne** pour afficher votre nouveau fragment de contenu.

### Créer des fragments de contenu de personne supplémentaires

Répétez les étapes ci-dessus pour créer d’autres fragments **Personne**.

1. Créez un fragment de contenu Personne pour **Alison Smith** avec les propriétés suivantes :

   * Emplacement : `/content/dam/my-project/en/people`.
   * Modèle de fragment de contenu : **Personne**
   * Titre : **Alison Smith**
   * Nom : `alison-smith`.

   Sélectionnez **Créer et ouvrir** et créez les valeurs suivantes :

   * Nom complet : **Alison Smith**
   * Biographie : **Alison est photographe et adore écrire sur ses voyages.**
   * Photo de profil : sélectionnez une image dans `/content/dam` ou chargez-en une nouvelle.
   * Profession : **Photographe**, **Voyageur**, **Écrivain**.

Vous devriez maintenant avoir deux fragments de contenu dans le dossier **Mon projet > Français > Personnes** :

![Nouveaux fragments de contenu.](assets/2/people-content-fragments.png)

Vous pouvez éventuellement créer d’autres fragments de contenu de personne pour représenter d’autres personnes.

## Créer un fragment de contenu d’équipe

En suivant la même approche, créez un fragment **Équipe** basé sur le modèle de fragment de contenu **Équipe** dans le dossier **Mon projet > Français > Équipes**.

1. Créez un fragment **Équipe** représentant **Équipe Alpha** avec les propriétés suivantes :

   * Emplacement : `/content/dam/my-project/en`.
   * Modèle de fragment de contenu : **Équipe**
   * Titre : **Équipe Alpha**
   * Nom : `team-alpha`.

   Sélectionnez **Créer et ouvrir** et créez les valeurs suivantes :

   * Titre : **Équipe Alpha**
   * Description : **Team Alpha est une équipe de contenu de voyage spécialisée dans la photographie et l&#39;écriture de voyages.**
   * **Membres de l’équipe** : sélectionnez les fragments de contenu **John Doe** et **Alison Smith** pour renseigner le champ **Membres de l’équipe**.

   ![Fragment de contenu Team Alpha](assets/2/team-alpha-content-fragment.png)

1. Sélectionnez **Créer et ouvrir** pour créer le fragment de contenu d’équipe
1. Il doit y avoir un fragment de contenu sous **Mon projet > Français > Équipe** :

Vous devriez maintenant disposer d’un fragment de contenu **Équipe Alpha** dans le dossier **Mon projet > Anglais > Équipes** :

![Fragments de contenu d’équipe](assets/2/team-content-fragments.png)

Vous pouvez éventuellement créer une **Équipe Oméga** avec un ensemble de personnes différent.

## Publier des fragments de contenu

Pour rendre les fragments de contenu disponibles via les API ouvertes, publiez-les. La publication permet d’accéder aux fragments de contenu via :

* **Service de publication** - diffuse du contenu aux applications de production.
* **Service de prévisualisation** : sert du contenu pour prévisualiser les applications.

En règle générale, le contenu est d’abord publié dans le **service d’aperçu** et révisé sur une application d’aperçu avant d’être publié dans le **service de publication**. La publication vers le **service de publication** ne publie pas également vers le **service d’aperçu**. Vous devez publier séparément sur le **service d’aperçu**.

Dans ce tutoriel, nous allons publier sur le service de publication AEM, mais l’utilisation du service d’aperçu AEM est aussi facile que la modification de l’URL du service [AEM dans l’application React](./4-react-app.md)

1. Dans la console Fragments de contenu, recherchez le dossier **Mon projet > Français**.
1. Sélectionnez tous les fragments de contenu dans le dossier **Français**, qui affiche tous les fragments de contenu dans tous les sous-dossiers, puis sélectionnez **Publier > Maintenant** dans la barre d’actions supérieure.

   ![Sélectionner les fragments de contenu à publier](assets/2/select-publish-content-fragments.png)

1. Sélectionnez le **Service de publication**, sous **Inclure toutes les références** sélectionnez **Dépublié** et **Modifié**, puis sélectionnez **Publier**.

   ![Publication d’un fragment de contenu.](assets/2/publish-content-fragments.png)

Désormais, les fragments de contenu, ainsi que tous les fragments de contenu de personne référencés par les fragments de contenu d’équipe et toutes les ressources référencées, sont publiés sur le **service de publication**.

Vous pouvez publier sur le service **Aperçu** de la même manière.

## Félicitations.

Félicitations, vous avez correctement créé des fragments de contenu basés sur des modèles de fragment de contenu dans AEM. Vous avez créé un modèle de fragment de contenu **Personne**, créé plusieurs fragments de contenu **Personne** et créé un fragment de contenu **Équipe** qui fait référence à plusieurs fragments de contenu **Personne**.

Une fois les fragments de contenu publiés, vous pouvez désormais y accéder via la diffusion de fragments de contenu AEM avec les API OpenAPI.

## Étapes suivantes

Dans le chapitre suivant, [Explorer les API OpenAPI](3-explore-openapis.md), vous allez explorer la diffusion de fragments de contenu AEM avec les API OpenAPI à l’aide de la fonctionnalité **L’essayer** intégrée à la documentation de l’API.

