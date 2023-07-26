---
title: Personnalisation à l’aide d’Adobe Target
seo-title: Personalization using Adobe Target
description: Tutoriel complet montrant comment créer et diffuser une expérience personnalisée à l’aide d’Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Personnalisation des expériences de pages web complètes à l’aide d’Adobe Target

Dans le chapitre précédent, nous avons appris à créer une activité basée sur la géolocalisation dans Adobe Target à l’aide de contenu créé en tant que fragments d’expérience et exporté depuis AEM en tant qu’offres de HTML.

Dans ce chapitre, nous allons explorer la création d’une activité pour rediriger les pages de votre site hébergées sur AEM vers une nouvelle page à l’aide d’Adobe Target.

## Présentation du scénario

Le site WKND a repensé sa page d’accueil et souhaite rediriger les visiteurs de sa page d’accueil actuelle vers la nouvelle page d’accueil. En même temps, vous devez également comprendre comment la nouvelle page d’accueil contribue à améliorer l’engagement et les recettes des utilisateurs. En tant que marketeur, vous avez reçu la tâche de créer une activité pour rediriger les visiteurs vers la nouvelle page d’accueil. Explorons la page d’accueil du site WKND et apprenons à créer une activité à l’aide d’Adobe Target.

### Utilisateurs impliqués

Pour cet exercice, les utilisateurs suivants doivent être impliqués et effectuer certaines tâches nécessitant un accès administratif.

* **Producteur de contenu/éditeur de contenu** (Adobe Experience Manager)
* **Marketer** (Adobe Target/équipe d’optimisation)

### Page d’accueil du site WKND

![Scénario 1 AEM](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Conditions préalables

* **AEM**
   * [AEM instance de création et de publication](./implementation.md#getting-aem) s’exécutant sur localhost 4502 et 4503, respectivement.
   * [AEM intégré à Adobe Target à l’aide d’Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accès à vos organisations Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fourni avec les solutions suivantes
      * [Adobe Target](https://experiencecloud.adobe.com)

## Activités de l’éditeur de contenu

1. Le spécialiste du marketing lance la discussion de reconception de la page d’accueil WKND avec AEM Content Editor et détaille les exigences.
   * ***Condition requise*** : Reconcevez la page d’accueil du site WKND à l’aide d’une conception basée sur des cartes.
2. En fonction des exigences, AEM Éditeur de contenu crée ensuite une page d’accueil du site WKND avec une conception par carte et publie la nouvelle page d’accueil.

## Activités marketing

1. Le marketeur crée une activité de ciblage A/B avec l’offre de redirection comme expérience et un trafic de site web à 100 % affecté à la nouvelle page d’accueil avec un objectif de succès et des mesures ajoutées.
   1. Dans la fenêtre Adobe Target, accédez à **Activités** .
   2. Cliquez sur **Création d’une activité** et sélectionnez le type d’activité comme **Test A/B**
      ![Adobe Target - Création d’une activité](assets/personalization-use-case-2/create-ab-activity.png)
   3. Sélectionnez la **Web** et choisissez le canal **Compositeur d’expérience visuelle**.
   4. Saisissez le **URL d’activité** et cliquez sur **Suivant** pour ouvrir le compositeur d’expérience visuelle.
      ![Adobe Target - Création d’une activité](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Pour **Compositeur d’expérience visuelle** pour charger, activez **Autoriser le chargement de scripts non sécurisés** dans votre navigateur et rechargez votre page.
      ![Activité de ciblage d’expérience](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. La page d’accueil du site WKND s’ouvre dans l’éditeur du compositeur d’expérience visuelle.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Survol **Expérience B** et sélectionnez afficher d’autres options.
      ![Expérience B](assets/personalization-use-case-2/redirect-url.png)
   8. Sélectionner **Rediriger vers l’URL** et saisissez l’URL de la nouvelle page d’accueil WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Expérience B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Enregistrer** vos modifications et passez aux étapes suivantes de la création de l’activité.
   10. Sélectionnez la **Méthode d’affectation du trafic** comme manuel et affecter 100 % du trafic à **Expérience B**.
      ![Trafic de l’expérience B](assets/personalization-use-case-2/traffic.png)
   11. Cliquez sur **Suivant**.
   12. Fournir **Mesures d’objectif** pour votre activité et enregistrez et fermez votre test A/B.
      ![Mesure de l’objectif de test A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Attribuez un nom (**Reconception de la page d’accueil WKND**) pour votre activité et enregistrez vos modifications.
   14. Dans l’écran Détails de l’activité, veillez à **Activer** votre activité.
      ![Activer l’activité](assets/personalization-use-case-2/ab-activate.png)
   15. Accédez à la page d’accueil WKND (http://localhost:4503/content/wknd/en.html) et vous êtes redirigé vers la page d’accueil du site WKND repensée (http://localhost:4503/content/wknd/en1.html).
      ![Page d’accueil WKND repensée](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Résumé

Dans ce chapitre, un spécialiste du marketing a pu créer une activité pour rediriger les pages de votre site hébergées sur AEM vers une nouvelle page à l’aide d’Adobe Target.
