---
title: Personnalisation à l’aide du compositeur d’expérience visuelle
description: Découvrez comment créer une activité Adobe Target à l’aide du compositeur d’expérience visuelle.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 2%

---

# Personnalisation à l’aide du compositeur d’expérience visuelle {#personalization-vec}

Découvrez comment créer une activité Target de test A/B à l’aide du compositeur d’expérience visuelle (VEC).

## Prérequis

Pour utiliser le compositeur d’expérience visuelle sur un site web AEM, la configuration suivante doit être effectuée :

1. [Ajout d’Adobe Target à votre site web AEM](./add-target-launch-extension.md)
1. [Déclencher un appel Adobe Target à partir de Launch](./load-and-fire-target.md)

## Présentation du scénario

La page d’accueil du site WKND présente les activités locales ou les meilleures choses à faire autour d’une ville sous la forme de cartes d’information. En tant que marketeur, vous avez reçu la tâche de modifier la page d’accueil, en apportant des modifications au texte du teaser de section d’aventure et en sachant comment cela améliore la conversion.

## Procédure de création d’un test A/B à l’aide du compositeur d’expérience visuelle (VEC)

1. Connectez-vous à [Adobe Experience Cloud](https://experience.adobe.com/), appuyez sur . __Cible__, accédez au __Activités__ tab

   + Si vous ne voyez pas __Cible__ dans le tableau de bord de l’Experience Cloud, assurez-vous que la bonne organisation d’Adobe est sélectionnée dans le sélecteur d’organisation en haut à droite et que l’accès à Target a été accordé à votre utilisateur dans [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Cliquez sur **Création d’une activité** puis choisissez **Test A/B** activité

   ![Activité A/B](assets/ab-target-activity.png)

1. Sélectionnez la **Compositeur d’expérience visuelle** , indiquez l’URL d’activité, puis cliquez sur **Suivant**

   ![URL d’activité](assets/ab-test-url.png)

1. Le compositeur d’expérience visuelle affiche deux onglets sur le côté gauche après la création d’une activité : *Expérience A* et *Expérience B*. Sélectionnez une expérience dans la liste. Vous pouvez ajouter de nouvelles expériences à la liste à l’aide du **Ajout d’une expérience** bouton .

   ![Expérience A](assets/experience.png)

1. Sélectionnez une image ou du texte sur la page pour commencer à apporter des modifications. Vous pouvez également utiliser l’éditeur de code pour sélectionner et HTML l’élément .

   ![Élément](assets/select-element.png)

1. Modifier le texte à partir de *Camping en Australie occidentale* to *Aventures d&#39;Australie*. Une liste des modifications ajoutées à une expérience s’affiche sous Modifications. Vous pouvez cliquer et modifier l’élément modifié pour afficher son sélecteur CSS et le nouveau contenu qui y est ajouté.

   ![Aventures](assets/adventures.png)

1. Renommer *Expérience A* to *Adventure*
1. De même, mettez à jour le texte sur *Expérience B* de *Camping en Australie occidentale* to *Exploration de la nature australienne*.

   ![Exploration](assets/explore.png)

1. Cliquez sur **Suivant** pour passer à Ciblage et conservons une affectation manuelle du trafic de 50 à 50 entre les deux expériences.

   ![Ciblage](assets/targeting.png)

1. Pour les objectifs et les paramètres, choisissez la source de création de rapports comme Adobe Target et sélectionnez la mesure Objectif comme Conversion avec une action de page vue.

   ![Objectifs](assets/goals.png)

1. Attribuez un nom à votre activité et enregistrez.
1. Activez votre activité enregistrée pour mettre vos modifications en ligne.

   ![Objectifs](assets/activate.png)

1. Ouvrez la page de votre site (URL d’activité de l’étape 3) dans un nouvel onglet et vous devriez pouvoir visualiser l’une des expériences (aventure ou exploration) de notre activité de test A/B.

   ![Objectifs](assets/publish.png)

## Résumé

Dans ce chapitre, un marketeur a pu créer une expérience à l’aide du compositeur d’expérience visuelle en faisant glisser et en déposant, en permutant et en modifiant la mise en page et le contenu d’une page web sans modifier de code pour exécuter un test.

## Liens pris en charge

+ [Débogueur Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Débogueur Adobe Experience Cloud - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
