---
title: Personnaliser l’expérience complète d’une page web
description: Découvrez comment créer une activité Target pour rediriger vos pages de site web AEM vers de nouvelles pages à l’aide d’Adobe Target.
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 126
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 100%

---

# Personnaliser l’expérience complète d’une page web {#personalization-fpe}

Découvrez comment créer une activité pour rediriger les pages de votre site hébergées sur AEM vers une nouvelle page à l’aide d’Adobe Target.

## Conditions préalables

Pour personnaliser les pages complètes d’un site web AEM, la configuration suivante doit être effectuée :

1. [Ajouter Adobe Target à votre site web AEM](./add-target-launch-extension.md)
1. [Déclencher un appel Adobe Target à partir de Launch](./load-and-fire-target.md)

## Vue d’ensemble du scénario

Le site WKND a repensé sa page d’accueil et souhaite rediriger les visiteurs et les visiteuses de sa page d’accueil actuelle vers la nouvelle page d’accueil. Vous devez également comprendre comment la nouvelle page d’accueil contribue à améliorer l’engagement des utilisateurs et utilisatrices et le chiffre d’affaires. En tant que personne spécialiste marketing, on vous a confié la tâche de créer une activité pour rediriger les visiteurs et les visiteuses vers la nouvelle page d’accueil. Explorons la page d’accueil du site WKND et apprenons à créer une activité à l’aide d’Adobe Target.

## Procédure de création d’un test A/B à l’aide du compositeur d’expérience visuelle (VEC)

1. Connectez-vous à Adobe Target et accédez à l’onglet Activités.
1. Cliquez sur le bouton **Créer une activité** puis choisissez l’activité **Test A/B**.

   ![Activité A/B.](assets/ab-target-activity.png)

1. Sélectionnez l’option **Compositeur d’expérience visuelle**, indiquez l’URL d’activité, puis cliquez sur **Suivant**.

   ![URL d’activité.](assets/ab-test-url.png)

1. Le compositeur d’expérience visuelle affiche deux onglets sur le côté gauche après la création d’une activité : *Expérience A* et *Expérience B*. Sélectionnez une expérience dans la liste. Vous pouvez ajouter de nouvelles expériences à la liste à l’aide du bouton **Ajouter une expérience**.

   ![Options d’expérience.](assets/experience-options.png)

1. Affichez les options disponibles pour l’expérience A, puis sélectionnez l’option **Rediriger vers l’URL** et indiquez une URL pour la nouvelle page d’accueil du site WKND.

   ![URL de redirection.](assets/redirect-url.png)

1. Renommez *Expérience A* en *Nouvelle page d’accueil WKND* et *Expérience B* en *Page d’accueil WKND*.

   ![Adventures.](assets/new-experiences.png)

1. Cliquez sur **Suivant** pour passer au Ciblage et conservez une attribution manuelle du trafic à 50/50 entre les deux expériences.

   ![Ciblage.](assets/targeting.png)

1. Pour les objectifs et les paramètres, sélectionnez Adobe Target comme source de création de rapports et sélectionnez Conversion comme mesure Objectif avec une action de page vue.

   ![Objectifs.](assets/goals.png)

1. Attribuez un nom à votre activité et enregistrez.
1. Activez votre activité enregistrée pour mettre vos modifications en ligne.

   ![Objectifs.](assets/activate.png)

1. Ouvrez la page de votre site (URL d’activité de l’étape 3) dans un nouvel onglet puis affichez l’une des expériences (page d’accueil WKND ou nouvelle page d’accueil WKND) à partir de l’activité de test A/B. `us/en.html` redirige vers `us/home.html`.

   ![Objectifs.](assets/redirect-test.png)

## Résumé

En tant que personne professionnelle du marketing, vous avez été en mesure de créer une activité pour rediriger les pages de votre site hébergées sur AEM vers une nouvelle page à l’aide d’Adobe Target.

## Liens connexes

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/fr/firefox/addon/adobe-experience-platform-dbg/)
