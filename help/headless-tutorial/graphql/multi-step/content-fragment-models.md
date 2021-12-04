---
title: Définition de modèles de fragment de contenu - Prise en main d’AEM sans affichage - GraphQL
description: Prise en main d’Adobe Experience Manager (AEM) et de GraphQL. Découvrez comment modéliser du contenu et créer un schéma avec des modèles de fragment de contenu dans AEM. Examinez les modèles existants et créez un modèle. Découvrez les différents types de données qui peuvent être utilisés pour définir un schéma.
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 2%

---

# Définition de modèles de fragment de contenu {#content-fragment-models}

Dans ce chapitre, apprenez à modéliser du contenu et à créer un schéma avec **Modèles de fragment de contenu**. Vous allez passer en revue les modèles existants et créer un nouveau modèle. Vous découvrirez également les différents types de données qui peuvent être utilisés pour définir un schéma dans le cadre du modèle.

Dans ce chapitre, vous allez créer un modèle pour un **Contributeur**, qui est le modèle de données pour les utilisateurs qui créent du contenu de magazine et d’aventure dans le cadre de la marque WKND.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans la section [Configuration rapide](../quick-setup/local-sdk.md) ont été terminées.

## Objectifs {#objectives}

* Créez un modèle de fragment de contenu.
* Identifiez les types de données disponibles et les options de validation pour la création de modèles.
* Comprendre comment le modèle de fragment de contenu définit **both** le schéma de données et le modèle de création d’un fragment de contenu.

## Présentation du modèle de fragment de contenu {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

La vidéo ci-dessus donne un aperçu général de l’utilisation des modèles de fragment de contenu.

>[!CAUTION]
>
> La vidéo ci-dessus montre la création de la variable **Contributeur** modèle avec le nom `Contributors`. Lors de l’exécution des étapes dans votre propre environnement, assurez-vous que le titre utilise le formulaire unique : `Contributor` sans le **s**. Le nommage du modèle de fragment de contenu entraîne les appels d’API GraphQL qui seront effectués ultérieurement dans le tutoriel.

## Inspect : modèle de fragment de contenu d’aventure

Dans le chapitre précédent, plusieurs fragments de contenu avancés ont été édités et affichés sur une application externe. Examinons le modèle de fragment de contenu aventure pour comprendre le schéma de données sous-jacent de ces fragments.

1. Dans la **AEM** Accédez à **Outils** > **Ressources** > **Modèles de fragment de contenu**.

   ![Accès aux modèles de fragment de contenu](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Accédez au **Site WKND** et placez le pointeur de la souris sur le dossier **Adventure** Modèle de fragment de contenu et cliquez sur le **Modifier** icône (crayon) pour ouvrir le modèle.

   ![Ouvrir le modèle de fragment de contenu Adventure](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Cela ouvre la fenêtre **Éditeur de modèle de fragment de contenu**. Notez que les champs définissent le modèle Adventure avec différents **Types de données** like **Texte sur une seule ligne**, **Texte multi-lignes**, **Énumération**, et **Référence de contenu**.

1. La colonne de droite de l’éditeur répertorie les **Types de données** qui définissent les champs de formulaire utilisés pour la création de fragments de contenu.

1. Sélectionnez la **Titre** dans le panneau principal. Dans la colonne de droite, cliquez sur le bouton **Propriétés** tab :

   ![Propriétés du titre de l’aventure](assets/content-fragment-models/adventure-title-properties-tab.png)

   Observez les **Nom de la propriété** est défini sur `adventureTitle`. Cela définit le nom de la propriété qui est conservée dans AEM. Le **Nom de la propriété** définit également la variable **key** nom de cette propriété dans le cadre du schéma de données. Ceci **key** sera utilisé lorsque les données de fragment de contenu sont exposées via les API GraphQL.

   >[!CAUTION]
   >
   > Modification de la variable **Nom de la propriété** d’un champ **after** Les fragments de contenu sont dérivés du modèle et ont des effets en aval. Les valeurs de champ des fragments existants ne sont plus référencées et le schéma de données exposé par GraphQL change, ce qui affecte les applications existantes.

1. Faites défiler l’écran vers le bas **Propriétés** et affichez le **Type de validation** menu déroulant.

   ![Options de validation disponibles](assets/content-fragment-models/validation-options-available.png)

   Des validations de formulaire prêtes à l’emploi sont disponibles pour **Courrier électronique** et **URL**. Il est également possible de définir une **Personnalisé** validation à l’aide d’une expression régulière.

1. Cliquez sur **Annuler** pour fermer l’éditeur de modèle de fragment de contenu.

## Création d’un modèle de contributeur

Créez ensuite un modèle pour un **Contributeur**, qui est le modèle de données pour les utilisateurs qui créent du contenu de magazine et d’aventure dans le cadre de la marque WKND.

1. Cliquez sur **Créer** dans le coin supérieur droit pour afficher le **Créer un modèle** assistant.
1. Pour **Titre du modèle** enter : **Contributeur** et cliquez sur **Créer**

   ![Assistant de modèle de fragment de contenu](assets/content-fragment-models/content-fragment-model-wizard.png)

   Cliquez sur **Ouvrir** pour ouvrir le modèle nouvellement créé.

1. Faites glisser et déposez un **Texte sur une seule ligne** sur le panneau principal. Renseignez les propriétés suivantes sur le **Propriétés** tab :

   * **Libellé du champ**: **Nom complet**
   * **Nom de la propriété**: `fullName`
   * Vérifier **Obligatoire**

   ![Champ de propriété Full Name](assets/content-fragment-models/full-name-property-field.png)

1. Cliquez sur le bouton **Types de données** et effectuez un glisser-déposer d’un élément **Texte multi-lignes** sous le champ **Nom complet** champ . Renseignez les propriétés suivantes :

   * **Libellé du champ**: **Biographie**
   * **Nom de la propriété**: `biographyText`
   * **Type par défaut**: **Texte enrichi**

1. Cliquez sur le bouton **Types de données** et effectuez un glisser-déposer d’un élément **Référence de contenu** champ . Renseignez les propriétés suivantes :

   * **Libellé du champ**: **Référence d’image**
   * **Nom de la propriété**: `pictureReference`
   * **Chemin racine**: `/content/dam/wknd`

   Lors de la configuration de la variable **Chemin racine** vous pouvez cliquer sur le bouton **folder** pour afficher un modal afin de sélectionner le chemin. Cela permet de restreindre les dossiers que les auteurs peuvent utiliser pour renseigner le chemin.

   ![Chemin racine configuré](assets/content-fragment-models/root-path-configure.png)

1. Ajoutez une validation au **Référence d’image** de sorte que seuls les types de contenu de **Images** peut être utilisé pour remplir le champ.

   ![Limitation aux images](assets/content-fragment-models/picture-reference-content-types.png)

1. Cliquez sur le bouton **Types de données** et effectuez un glisser-déposer d’un élément **Énumération**  type de données sous le **Référence d’image** champ . Renseignez les propriétés suivantes :

   * **Libellé du champ**: **Profession**
   * **Nom de la propriété**: `occupation`

1. Ajouter plusieurs **Options** en utilisant la variable **Ajouter une option** bouton . Utiliser la même valeur pour **Étiquette d’option** et **Valeur de l’option**:

   **Artiste**, **Influenceur**, **Photographe**, **Voyageur**, **Écrivain**, **YouTuber**

   ![Valeurs des options d’occupation](assets/content-fragment-models/occupation-options-values.png)

1. La finale **Contributeur** modèle doit se présenter comme suit :

   ![Modèle de contributeur final](assets/content-fragment-models/final-contributor-model.png)

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.

## Activation du modèle du contributeur

Les modèles de fragment de contenu doivent être **Activé** avant que les auteurs de contenu puissent l’utiliser. Il est possible de **Désactiver** un modèle de fragment de contenu, interdisant ainsi aux auteurs de l’utiliser. Rappelez-vous que la modification de **Nom de la propriété** d’un champ du modèle modifie le schéma de données sous-jacent et peut avoir des effets significatifs en aval sur les fragments existants et les applications externes. Il est recommandé de planifier soigneusement la convention d’affectation des noms utilisée pour la variable **Nom de la propriété** de champs avant d’activer le modèle de fragment de contenu pour les utilisateurs.

1. Assurez-vous que la variable **Contributeur** Le modèle se trouve actuellement dans une **Activé** état.

   ![Modèle de contributeur activé](assets/content-fragment-models/enable-contributor-model.png)

   Il est possible de basculer l’état d’un modèle de fragment de contenu en faisant glisser le curseur sur la carte et en cliquant sur l’icône **Désactiver** / **Activer** icône .

## Félicitations ! {#congratulations}

Félicitations, vous venez de créer votre premier modèle de fragment de contenu !

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Création de modèles de fragment de contenu](author-content-fragments.md), vous allez créer et modifier un fragment de contenu en fonction d’un modèle de fragment de contenu. Vous apprendrez également à créer des variantes de fragments de contenu.
