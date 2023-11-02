---
title: Personnalisation à l’aide du compositeur d’expérience visuelle
description: Découvrez comment créer une activité Adobe Target à l’aide du compositeur d’expérience visuelle.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 100%

---

# Personnalisation à l’aide du compositeur d’expérience visuelle {#personalization-vec}

Découvrez comment créer une activité Target de test A/B à l’aide du compositeur d’expérience visuelle (VEC).

## Conditions préalables

Pour utiliser le compositeur d’expérience visuelle sur un site web AEM, la configuration suivante doit être effectuée :

1. [Ajouter Adobe Target à votre site web AEM](./add-target-launch-extension.md)
1. [Déclencher un appel Adobe Target à partir de Launch](./load-and-fire-target.md)

## Vue d’ensemble du scénario

La page d’accueil du site WKND présente les activités locales ou les meilleures choses à faire dans une ville sous la forme de cartes d’information. En tant que personne chargée du marketing, vous avez pour tâche de modifier la page d’accueil en apportant des modifications au texte du teaser de l’aventure en gardant à l’esprit comment améliorer les conversions.

## Procédure de création d’un test A/B à l’aide du compositeur d’expérience visuelle (VEC)

1. Connectez-vous à [Adobe Experience Cloud](https://experience.adobe.com/), appuyez sur __Target__ et accédez à l’onglet __Activités__.

   + Si vous ne trouvez pas __Target__ dans le tableau de bord d’Experience Cloud, assurez-vous que la bonne organisation Adobe apparaît dans le sélecteur d’organisation en haut à droite et que l’accès à Target a été accordé à votre utilisateur ou utilisatrice dans l’[Adobe Admin Console](https://adminconsole.adobe.com/).

1. Cliquez sur le bouton **Créer une activité** puis choisissez l’activité **Test A/B**.

   ![Activité A/B.](assets/ab-target-activity.png)

1. Sélectionnez l’option **Compositeur d’expérience visuelle**, indiquez l’URL d’activité, puis cliquez sur **Suivant**.

   ![URL d’activité.](assets/ab-test-url.png)

1. Le compositeur d’expérience visuelle affiche deux onglets sur le côté gauche après la création d’une activité : *Expérience A* et *Expérience B*. Sélectionnez une expérience dans la liste. Vous pouvez ajouter de nouvelles expériences à la liste à l’aide du bouton **Ajouter une expérience**.

   ![Expérience A.](assets/experience.png)

1. Sélectionnez une image ou du texte sur la page pour commencer à apporter des modifications. Vous pouvez également utiliser l’éditeur de code pour sélectionner un élément HTML.

   ![Élément.](assets/select-element.png)

1. Modifiez le texte *Camping in Western Australia* en *Adventures of Australia*. La liste des modifications ajoutées à une expérience s’affiche sous Modifications. Vous pouvez cliquer et modifier l’élément modifié pour afficher son sélecteur CSS et le nouveau contenu qui y est ajouté.

   ![Adventures.](assets/adventures.png)

1. Renommez *Expérience A* en *Adventure*.
1. De même, mettez à jour le texte dans *Expérience B* pour passer de *Camping in Western Australia* à *Explore the Australian Wilderness*.

   ![Explore.](assets/explore.png)

1. Cliquez sur **Suivant** pour passer au ciblage. Conservez une affectation manuelle du trafic de 50-50 entre les deux expériences.

   ![Ciblage.](assets/targeting.png)

1. Pour les objectifs et les paramètres, sélectionnez Adobe Target comme source de création de rapports et sélectionnez Conversion comme mesure Objectif avec une action de page vue.

   ![Objectifs.](assets/goals.png)

1. Attribuez un nom à votre activité et enregistrez.
1. Activez votre activité enregistrée pour mettre vos modifications en ligne.

   ![Objectifs.](assets/activate.png)

1. Ouvrez la page de votre site (URL d’activité de l’étape 3) dans un nouvel onglet. Vous devriez pouvoir visualiser l’une des expériences (Adventure ou Explore) de notre activité de test A/B.

   ![Objectifs.](assets/publish.png)

## Résumé

Dans ce chapitre, une personne chargée du marketing a pu créer une expérience à l’aide du compositeur d’expérience visuelle en faisant glisser et en déposant du contenu, mais aussi en modifiant la disposition d’une page web sans changer le code pour exécuter un test.

## Liens connexes

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/fr/firefox/addon/adobe-experience-platform-dbg/)
