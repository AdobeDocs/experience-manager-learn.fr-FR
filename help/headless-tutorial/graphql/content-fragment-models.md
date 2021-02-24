---
title: Définition de modèles de fragments de contenu - Pour commencer avec AEM sans en-tête - GraphQL
description: Commencez avec Adobe Experience Manager (AEM) et GraphQL. Découvrez comment modéliser du contenu et créer un schéma avec des modèles de fragments de contenu dans AEM. Examinez les modèles existants et créez un nouveau modèle. Découvrez les différents types de données qui peuvent être utilisés pour définir un schéma.
sub-product: ressources
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
translation-type: tm+mt
source-git-commit: ce4a35f763862c6d6a42795fd5e79d9c59ff645a
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 2%

---


# Définition de modèles de fragments de contenu {#content-fragment-models}

Dans ce chapitre, apprenez à modéliser le contenu et à créer un schéma avec **Modèles de fragment de contenu**. Vous passerez en revue les modèles existants et créerez un nouveau modèle. Vous découvrirez également les différents types de données qui peuvent être utilisés pour définir un schéma dans le cadre du modèle.

Dans ce chapitre, vous allez créer un nouveau modèle pour un **contributeur**, qui est le modèle de données pour les utilisateurs qui créent des magazines et du contenu d&#39;aventure dans le cadre de la marque WKND.

## Conditions préalables {#prerequisites}

Il s’agit d’un didacticiel en plusieurs parties et on suppose que les étapes décrites dans la section [Configuration rapide](./setup.md) ont été terminées.

## Objectifs {#objectives}

* Créez un modèle de fragment de contenu.
* Identifiez les types de données disponibles et les options de validation pour la création de modèles.
* Comprenez comment le modèle de fragment de contenu définit **à la fois** le schéma de données et le modèle de création d’un fragment de contenu.

## Présentation du modèle de fragment de contenu {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

La vidéo ci-dessus donne un aperçu général de l’utilisation des modèles de fragments de contenu.

## Inspect : modèle de fragment de contenu d&#39;aventure

Dans le chapitre précédent, plusieurs fragments de contenu Adventures ont été modifiés et affichés sur une application externe. Examinons le modèle de fragment de contenu incitatif pour comprendre le schéma de données sous-jacent de ces fragments.

1. Dans le menu **AEM Début**, accédez à **Outils** > **Ressources** > **Modèles de fragments de contenu**.

   ![Accédez aux modèles de fragments de contenu.](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Accédez au dossier **WKND Site** et passez la souris sur **Adventure** Content Fragment Model et cliquez sur l&#39;icône **Modifier** (crayon) pour ouvrir le modèle.

   ![Ouvrir le modèle de fragment de contenu d&#39;aventure](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Cela ouvre l&#39;**éditeur de modèle de fragment de contenu**. Observez que les champs définissent le modèle Aventure avec des **types de données** différents, tels que **texte sur une seule ligne**, **texte sur plusieurs lignes**, **Énumération** et **Référence du contenu**.

1. La colonne de droite de l’éditeur liste les **types de données** disponibles qui définissent les champs de formulaire utilisés pour la création de fragments de contenu.

1. Sélectionnez le champ **Titre** dans le panneau principal. Dans la colonne de droite, cliquez sur l&#39;onglet **Propriétés** :

   ![Propriétés du titre de l&#39;aventure](assets/content-fragment-models/adventure-title-properties-tab.png)

   Observez que le champ **Nom de la propriété** est défini sur `adventureTitle`. Définit le nom de la propriété conservée à l’AEM. Le **nom de la propriété** définit également le nom **key** pour cette propriété dans le cadre du schéma de données. Cette **clé** sera utilisée lorsque les données du fragment de contenu sont exposées via les API GraphQL.

   >[!CAUTION]
   >
   > La modification du **nom de propriété** d&#39;un champ **après que les fragments de contenu** aient été dérivés du modèle, a des effets en aval. Les valeurs de champ des fragments existants ne seront plus référencées et le schéma de données exposé par GraphQL changera, ce qui aura un impact sur les applications existantes.

1. Faites défiler l&#39;écran vers le bas dans l&#39;onglet **Propriétés** et vue la liste déroulante **Type de validation**.

   ![Options de validation disponibles](assets/content-fragment-models/validation-options-available.png)

   Les validations de formulaire prêtes à l’emploi sont disponibles pour **E-mail** et **URL**. Il est également possible de définir une validation **personnalisée** à l’aide d’une expression régulière.

1. Cliquez sur **Annuler** pour fermer l&#39;éditeur de modèle de fragment de contenu.

## Créer un modèle de collaboration

Ensuite, créez un nouveau modèle pour un **contributeur**, qui est le modèle de données pour les utilisateurs qui créent des magazines et du contenu d&#39;aventure dans le cadre de la marque WKND.

1. Cliquez sur **Créer** dans le coin supérieur droit pour afficher l&#39;Assistant **Créer un modèle**.
1. Pour **Titre du modèle**, saisissez : **Contributeur** et cliquez sur **Créer**

   ![Assistant de modèle de fragment de contenu](assets/content-fragment-models/content-fragment-model-wizard.png)

   Cliquez sur **Ouvrir** pour ouvrir le modèle nouvellement créé.

1. Faites glisser un élément **Texte sur une seule ligne** vers le panneau principal. Entrez les propriétés suivantes dans l&#39;onglet **Propriétés** :

   * **Libellé** du champ :  **Nom complet**
   * **Nom de la propriété**: `fullName`
   * Vérifier **Obligatoire**

   ![Champ de propriété Nom complet](assets/content-fragment-models/full-name-property-field.png)

1. Cliquez sur l&#39;onglet **Types de données** et faites glisser et déposez un champ **Texte à plusieurs lignes** sous le champ **Nom complet**. Saisissez les propriétés suivantes :

   * **Libellé** du champ :  **Biographie**
   * **Nom de la propriété**: `biographyText`
   * **Type** par défaut :  **Texte enrichi**

1. Cliquez sur l&#39;onglet **Types de données** et faites glisser un champ **Référence du contenu**. Saisissez les propriétés suivantes :

   * **Libellé** du champ :  **Référence de l&#39;image**
   * **Nom de la propriété**: `pictureReference`
   * **Chemin racine**: `/content/dam/wknd`

   Lors de la configuration du **chemin d’accès racine**, vous pouvez cliquer sur l’icône **dossier** pour afficher un canal permettant de sélectionner le chemin d’accès. Les dossiers que les auteurs peuvent utiliser pour renseigner le chemin seront ainsi restreints.

   ![Chemin racine configuré](assets/content-fragment-models/root-path-configure.png)

1. Ajoutez une validation de la **référence d&#39;image** afin que seuls les types de contenu **Images** puissent être utilisés pour remplir le champ.

   ![Limiter aux images](assets/content-fragment-models/picture-reference-content-types.png)

1. Cliquez sur l&#39;onglet **Types de données** et faites glisser et déposez un type de données **Énumération** sous le champ **Référence de l&#39;image**. Saisissez les propriétés suivantes :

   * **Libellé** du champ :  **Profession**
   * **Nom de la propriété**: `occupation`

1. Ajoutez plusieurs **options** à l&#39;aide du bouton **Ajouter une option**. Utilisez la même valeur pour **Option Label** et **Option Value** :

   **Artiste**,  **Influenceur**,  **Photographe**,  **Voyageur**, Writer, YouTuber ********

   ![Valeurs des options d’occupation](assets/content-fragment-models/occupation-options-values.png)

1. Le modèle final **Contributeur** doit se présenter comme suit :

   ![Modèle final du contributeur](assets/content-fragment-models/final-contributor-model.png)

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.

## Activer le modèle de contributeur

Les modèles de fragments de contenu doivent être **activés** avant que les auteurs de contenu puissent les utiliser. Il est possible de **désactiver** un modèle de fragment de contenu, interdisant ainsi aux auteurs de l’utiliser. Rappelez-vous que la modification du **nom de propriété** d&#39;un champ du modèle modifie le schéma de données sous-jacent et peut avoir des effets en aval importants sur les fragments existants et les applications externes. Il est recommandé de planifier soigneusement la convention d’affectation de nom utilisée pour le **nom de la propriété** des champs avant d’activer le modèle de fragment de contenu pour les utilisateurs.

1. Assurez-vous que le modèle **Contributeur** est actuellement dans un état **Activé**.

   ![Modèle Contributeur activé](assets/content-fragment-models/enable-contributor-model.png)

   Il est possible de basculer l’état d’un modèle de fragment de contenu en passant la souris sur la carte et en cliquant sur l’icône **Désactiver** / **Activer**.

## Félicitations! {#congratulations}

Félicitations, vous venez de créer votre premier modèle de fragment de contenu !

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Création de modèles de fragments de contenu](author-content-fragments.md), vous allez créer et modifier un nouveau fragment de contenu basé sur un modèle de fragment de contenu. Vous apprendrez également à créer des variantes de fragments de contenu.