---
title: Créer des modèles de fragment de contenu - Concepts avancés d’AEM Headless - GraphQL
description: Dans ce chapitre de Concepts avancés d’Adobe Experience Manager (AEM) Headless, apprenez à modifier un modèle de fragment de contenu en ajoutant des espaces réservés d’onglet, la date et l heure, des objets JSON, des références de fragment et des références de contenu.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 2122ab13-f9df-4f36-9c7e-8980033c3b10
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 100%

---

# Créer des modèles de fragment de contenu {#create-content-fragment-models}

Ce chapitre présente les étapes de la création de cinq modèles de fragment de contenu :

* **Informations de contact**
* **Adresse**
* **Personne**
* **Emplacement**
* **Équipe**

Les modèles de fragment de contenu permettent de définir des relations entre les types de contenu et de conserver ces dernières sous forme de schémas. Utilisez des références de fragments imbriqués, divers types de données de contenu et le type d’onglet pour l’organisation visuelle du contenu. Certains types de données sont plus avancés, tels que les espaces réservés d’onglet, les références de fragments, les objets JSON ainsi que le type de données date et heure.

Ce chapitre traite également de la manière d’améliorer les règles de validation pour les références de contenu telles que les images.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel avancé. Avant de passer à ce chapitre, veuillez vous assurer que vous avez terminé la [configuration rapide](../quick-setup/cloud-service.md). Assurez-vous que vous avez également lu le chapitre de [présentation](../overview.md) précédent pour plus d’informations sur la configuration du tutoriel avancé.

## Objectifs {#objectives}

* Créer des modèles de fragment de contenu.
* Ajouter aux modèles des espaces réservés d’onglet, la date et l’heure, des objets JSON, des références de fragments et des références de contenu.
* Ajouter la validation aux références de contenu.

## Présentation du modèle de fragment de contenu {#content-fragment-model-overview}

La vidéo suivante présente brièvement les modèles de fragment de contenu et la manière dont ils sont utilisés dans ce tutoriel.

>[!VIDEO](https://video.tv.adobe.com/v/340037?quality=12&learn=on)

## Créer des modèles de fragment de contenu {#create-models}

Créons des modèles de fragment de contenu pour l’application WKND. Si vous avez besoin d’une introduction générale à la création de modèles de fragment de contenu, veuillez consulter le chapitre approprié dans le [tutoriel de base](../multi-step/content-fragment-models.md).

1. Accédez à **Outils** > **Général** > **Modèles de fragment de contenu**.

   ![Chemin d’accès des modèles de fragment de contenu.](assets/define-content-fragment-models/content-fragment-models-path.png)

1. Sélectionnez **WKND Shared** pour afficher la liste des modèles de fragment de contenu existants pour le site.

### Modèle d’informations de contact {#contact-info-model}

Ensuite, créez un modèle qui contient les informations de contact d’une personne ou d’un lieu.

1. Sélectionnez **Créer** dans le coin supérieur droit.

1. Attribuez au modèle le titre « Informations de contact », puis sélectionnez **Créer**. Dans la boîte de dialogue modale qui apparaît, sélectionnez **Ouvrir** pour modifier le modèle nouvellement créé.

1. Commencez par faire glisser un champ de **texte monoligne** sur le modèle. Donnez-lui un **Libellé de champ** « Téléphone » dans l’onglet **Propriétés**. Le nom de la propriété est automatiquement rempli avec `phone`. Cochez la case pour que le champ soit **Obligatoire**.

1. Accédez à l’onglet **Types de données**, puis ajoutez un autre champ de **texte monoligne** sous le champ « Téléphone ». Donnez-lui un **Libellé de champ** « E-mail » et définissez-le également sur **Obligatoire**.

Adobe Experience Manager est fourni avec des méthodes de validation intégrées. Ces méthodes de validation vous permettent d’ajouter des règles de gouvernance à des champs spécifiques dans vos modèles de fragment de contenu. Dans ce cas, nous allons ajouter une règle de validation pour nous assurer que seules des adresses e-mail valides peuvent être saisies lorsque ce champ est renseigné. Sous le menu déroulant **Type de validation**, sélectionnez **E-mail**.

Votre modèle de fragment de contenu terminé devrait ressembler à ceci :

![Chemin d’accès du modèle d’informations de contact.](assets/define-content-fragment-models/contact-info-model.png)

Une fois que vous avez terminé, sélectionnez **Enregistrer** pour confirmer vos modifications et fermez l’éditeur de modèle de fragment de contenu.

### Modèle d’adresse {#address-model}

Créez ensuite un modèle pour une adresse.

1. À partir de **WKND Shared**, sélectionnez **Créer** dans le coin supérieur droit.

1. Saisissez le titre « Adresse », puis sélectionnez **Créer**. Dans la boîte de dialogue modale qui apparaît, sélectionnez **Ouvrir** pour modifier le modèle nouvellement créé.

1. Faites glisser et déposez un champ de **texte monoligne** sur le modèle et donnez-lui un **Libellé de champ** « Adresse postale ». Le nom de la propriété est ensuite renseigné en tant que `streetAddress`. Cochez la case **Obligatoire**.

1. Répétez les étapes ci-dessus et ajoutez quatre autres champs de « texte monoligne » au modèle. Utilisez les libellés suivants :

   * Ville
   * État
   * Code postal
   * Pays

1. Sélectionnez **Enregistrer** pour conserver les modifications apportées au modèle d’adresse.

   Le modèle de fragment « Adresse » terminé devrait ressembler à ceci :
   ![Modèle d’adresse.](assets/define-content-fragment-models/address-model.png)

### Modèle de personne {#person-model}

Ensuite, créez un modèle qui contient des informations sur une personne.

1. Dans le coin supérieur droit, sélectionnez **Créer**.

1. Donnez au modèle le titre « Personne », puis sélectionnez **Créer**. Dans la boîte de dialogue modale qui apparaît, sélectionnez **Ouvrir** pour modifier le modèle nouvellement créé.

1. Commencez par faire glisser un champ de **texte monoligne** sur le modèle. Donnez-lui un **Libellé de champ** « Nom complet ». Le nom de la propriété est automatiquement renseigné en tant que `fullName`. Cochez la case pour que le champ soit **Obligatoire**.

   ![Options de nom complet.](assets/define-content-fragment-models/full-name.png)

1. Les modèles de fragments de contenu peuvent être référencés dans d’autres modèles. Accédez à l’onglet **Types de données**, puis faites un glisser-déposer du champ **Référence de fragment** et donnez-lui un libellé « Informations de contact ».

1. Dans l’onglet **Propriétés**, sous le champ **Modèles de fragment de contenu autorisés**, sélectionnez l’icône de dossier, puis choisissez le modèle de fragment **Informations de contact** créé précédemment.

1. Ajoutez un champ **Référence de contenu** et donnez-lui un **Libellé de champ** « Photo de profil ». Sélectionnez l’icône du dossier sous **Chemin d’accès racine** pour ouvrir la boîte de dialogue modale de sélection du chemin d’accès. Sélectionnez un chemin d’accès racine en sélectionnant **Contenu** > **Ressources**, puis en cochant la case **WKND Shared**. Utilisez le bouton **Sélectionner** en haut à droite pour enregistrer le chemin d’accès. Le chemin d’accès du texte final devrait indiquer `/content/dam/wknd-shared`.

   ![Chemin d’accès racine de la référence de contenu.](assets/define-content-fragment-models/content-reference-root-path.png)

1. Sous **Accepter uniquement les types de contenu spécifiés**, sélectionnez « Image ».

   ![Options d’image de profil.](assets/define-content-fragment-models/profile-picture.png)

1. Pour limiter la taille et les dimensions du fichier image, examinons certaines options de validation pour le champ de référence de contenu.

   Sous **Accepter uniquement la taille de fichier spécifiée**, sélectionnez « Inférieure ou égale à », et des champs supplémentaires apparaissent ci-dessous.
   ![Accepter uniquement la taille de fichier spécifiée.](assets/define-content-fragment-models/accept-specified-file-size.png)

1. Pour **Maximum**, saisissez « 5 », et pour **Sélectionner l’unité**, sélectionnez « Mégaoctets (Mo) ». Cette validation permet de choisir uniquement les images à la taille spécifiée.

1. Sous **Accepter uniquement la largeur d’image spécifiée**, sélectionnez « Largeur maximale ». Dans le champ **Maximum (pixels)** qui apparaît, saisissez « 10 000 ». Sélectionnez les mêmes options pour **Accepter uniquement la hauteur d’image spécifiée**.

   Ces validations garantissent que les images ajoutées ne dépassent pas les valeurs spécifiées. Les règles de validation devraient désormais ressembler à ceci :

   ![Règles de validation des références de contenu](assets/define-content-fragment-models/content-reference-validation.png).

1. Ajoutez un champ de **texte multiligne** et donnez-lui un **Libellé de champ** « Biographie ». Laissez le menu déroulant **Type par défaut** comme option par défaut pour « Texte enrichi ».

   ![Options de biographie.](assets/define-content-fragment-models/biography.png)

1. Accédez à l’onglet **Types de données**, puis faites glisser un champ **Énumération** sous « Biographie ». Au lieu de l’option par défaut **Rendre en tant que**, sélectionnez **Menu déroulant** et donnez-lui un **Libellé de champ** « Niveau d’expérience de l’instructeur ou instructrice ». Saisissez une sélection d’options de niveau d’expérience de l’instructeur ou instructrice telles que _Expert, Avancé, Intermédiaire_.

1. Ensuite, faites glisser un autre champ **Énumération** sous « Niveau d’expérience de l’instructeur ou instructrice » et choisissez « cases à cocher » sous l’option **Rendre en tant que**. Donnez-lui le **Libellé de champ** « Compétences ». Saisissez différentes compétences telles que Rock Climbing, Surfing, Cycling, Skiing et Backpacking. Le libellé de l’option et la valeur de l’option doivent correspondre à ce qui suit :

   ![Énumération des compétences.](assets/define-content-fragment-models/skills-enum.png)

1. Pour finir, créez un libellé de champ « Détails de l’administrateur ou administratrice » en utilisant un champ de **texte multiligne**.

Sélectionnez **Enregistrer** pour confirmer vos modifications et fermez l’éditeur de modèle de fragment de contenu.

### Modèle d’emplacement {#location-model}

Le modèle de fragment de contenu suivant décrit un emplacement physique. Ce modèle utilise des espaces réservés d’onglet. Les espaces réservés d’onglet permettent d’organiser vos types de données dans l’éditeur de modèles et le contenu dans l’éditeur de fragments, respectivement, en catégorisant le contenu. Chaque espace réservé crée un onglet, semblable à un onglet dans un navigateur Internet, dans l’éditeur de fragment de contenu. Le modèle de localisation doit comporter deux onglets : Détails de l’emplacement et Adresse de l’emplacement.

1. Comme précédemment, sélectionnez **Créer** pour créer un autre modèle de fragment de contenu. Pour le titre du modèle, saisissez « Emplacement ». Sélectionnez **Créer**, puis **Ouvrir** dans la boîte de dialogue modale qui apparaît.

1. Ajoutez un champ **Espace réservé d’onglet** au modèle et libellez-le « Détails de l’emplacement ».

1. Glissez et déposez un champ de **texte monoligne** et libellez-le « Nom ». Sous le libellé de ce champ, ajoutez un champ de **texte multiligne** et libellez-le « Description ».

1. Ensuite, ajoutez un champ **Référence du segment** et libellez-le « Informations de contact ». Dans l’onglet des propriétés, sous **Modèles de fragment de contenu autorisés**, sélectionnez l’**Icône de dossier** et choisissez le modèle de fragment « Informations de contact » créé précédemment.

1. Ajoutez un champ **Référence de contenu** sous « Informations de contact ». Libellez-le « Image de l’emplacement ». Le **Chemin d’accès racine** doit être `/content/dam/wknd-shared.`. Sous **Accepter seulement les types de contenu spécifiés**, sélectionnez « Image ».

1. Ajoutons également un champ **Objet JSON** sous l’« Image de l’emplacement ». Dans la mesure où ce type de données est flexible, il peut être utilisé pour afficher toutes les données que vous souhaitez inclure dans votre contenu. Dans ce cas, l’objet JSON est utilisé pour afficher des informations sur la météo. Libellez l’objet JSON « Météo par saison ». Dans l’onglet **Propriétés**, ajoutez une **Description** afin que l’utilisateur ou l’utilisatrice sache clairement quelles données doivent être saisies ici : « Données JSON concernant la météo de l’emplacement de l’événement par saison (printemps, été, automne, hiver) ».

   ![Options d’objet JSON.](assets/define-content-fragment-models/json-object.png)

1. Pour créer l’onglet Adresse de l’emplacement, ajoutez un champ **Espace réservé d’onglet** au modèle et libellez-le « Adresse de l’emplacement ».

1. Faites un glisser-déposer d’un champ **Référence de fragment** et à partir de l’onglet Propriétés, libellez-le en tant qu’« Adresse », puis sous **Modèles de fragment de contenu autorisés**, sélectionnez le modèle **Adresse**.

1. Sélectionnez **Enregistrer** pour confirmer vos modifications et fermez l’éditeur de modèle de fragment de contenu. Le modèle Emplacement terminé devrait apparaître comme suit :

   ![Options de référence de contenu.](assets/define-content-fragment-models/location-model.png)

### Modèle d’équipe {#team-model}

Enfin, créez un modèle qui décrit une équipe de personnes.

1. À partir de la page **WKND Shared**, sélectionnez **Créer** pour créer un autre modèle de fragment de contenu. Pour le titre du modèle, saisissez « Équipe ». De la même manière que précédemment, sélectionnez **Créer** puis **Ouvrir** dans la boîte de dialogue modale qui apparaît.

1. Ajoutez un champ de **texte multiligne** au formulaire. Sous **Libellé du champ**, saisissez « Description ».

1. Ajoutez un champ **Date et heure** au modèle et libellez-le « Date de création de l’équipe ». Dans ce cas, gardez le **Type** par défaut défini sur « Date », mais sachez qu’il est également possible d’utiliser « Date et heure » ou « Heure ».

   ![Options de date et d’heure.](assets/define-content-fragment-models/date-and-time.png)

1. Accédez à l’onglet **Types de données**. Sous la « Date de création de l’équipe », ajoutez une **Référence de fragment**. Dans la liste déroulante **Afficher comme**, sélectionnez « texte multi-lignes ». Dans le **Libellé du champ**, saisissez « Membres de l’équipe ». Ce champ renvoie au modèle _Personne_ créé précédemment. Dans la mesure où le type de données est un champ multiple, plusieurs fragments Personne peuvent être ajoutés, ce qui permet de créer une équipe de personnes.

   ![Options de référence de fragment.](assets/define-content-fragment-models/fragment-reference.png)

1. Sous **Modèles de fragments de contenu autorisés**, utilisez l’icône du dossier pour ouvrir la boîte de dialogue modale Sélectionner le chemin d’accès, puis sélectionnez le modèle **Personne**. Utilisez le bouton **Sélectionner** pour enregistrer le chemin d’accès.

   ![Sélection du modèle de personne.](assets/define-content-fragment-models/select-person-model.png)

1. Sélectionnez **Enregistrer** pour confirmer vos modifications et fermez l’éditeur de modèle de fragment de contenu.

## Ajoutez des références de fragment au modèle d’Adventure {#fragment-references}

De la même manière que le modèle Équipe renvoie à un fragment du modèle Personne, les modèles Équipe et Emplacement doivent être référencés à partir du modèle Adventure pour afficher ces nouveaux modèles dans l’application WKND.

1. À partir de la page **WKND Shared**, sélectionnez le modèle **Adventure**, puis sélectionnez **Modifier** dans la barre de navigation supérieure.

   ![Chemin de modification d’Adventure.](assets/define-content-fragment-models/adventure-edit-path.png)

1. Au bas du formulaire, sous « What to Bring », ajoutez un champ **Référence de fragment**. Saisissez un **Libellé de champ** d’« Emplacement ». Sous **Modèles de fragment de contenu autorisés**, sélectionnez le modèle **Emplacement**.

   ![Options de référence de fragment d’emplacement.](assets/define-content-fragment-models/location-fragment-reference.png)

1. Ajoutez un autre champ **Référence de fragment** et libellez-le « Équipe d’instruction ». Sous **Modèles de fragment de contenu autorisés**, sélectionnez le modèle **Équipe**.

   ![Options de référence de fragment d’équipe.](assets/define-content-fragment-models/team-fragment-reference.png)

1. Ajoutez un autre champ **Référence de fragment** et libellez-le « Administration ».

   ![Options de référence de fragment d’administration.](assets/define-content-fragment-models/administrator-fragment-reference.png)

1. Sélectionnez **Enregistrer** pour confirmer vos modifications et fermez l’éditeur de modèle de fragment de contenu.

## Bonnes pratiques {#best-practices}

Il existe quelques bonnes pratiques relatives à la création de modèles de fragment de contenu :

* Créez des modèles qui mappent vers des composants UX. Par exemple, l’application WKND dispose de modèles de fragment de contenu pour les Adventures, les articles et l’emplacement. Vous pouvez également ajouter des en-têtes, des promotions ou des clauses de non-responsabilité. Chacun de ces exemples constitue un composant UX spécifique.

* Créez aussi peu de modèles que possible. Limiter le nombre de modèles vous permettra d’optimiser la réutilisation et la simplification de la gestion du contenu.

* Imbriquez les modèles de fragment de contenu aussi profondément que nécessaire, mais uniquement si besoin est. Rappelez-vous que l’imbrication s’effectue avec des références de fragment ou de contenu. Envisagez un maximum de cinq niveaux d’imbrication.

## Félicitations ! {#congratulations}

Félicitations. Vous avez maintenant ajouté des onglets, utilisé des types de données d’objet date et heure et JSON et vous en savez plus sur les références de fragment et de contenu. Vous avez également ajouté des règles de validation de référence de contenu.

## Étapes suivantes {#next-steps}

Le chapitre suivant de cette série couvre la [création de fragments de contenu](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md) à partir des modèles que vous avez créés dans ce chapitre. Découvrez comment utiliser les types de données introduits dans ce chapitre et comment créer des stratégies de dossier pour limiter les modèles de fragment de contenu pouvant être créés dans un dossier de ressources.

Bien que cela soit facultatif pour ce tutoriel, veillez à publier tout le contenu dans des situations de production réelles. Pour une révision des environnements de création et de publication dans AEM, reportez-vous à la
[Série de vidéos AEM Headless et GraphQL](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md).
