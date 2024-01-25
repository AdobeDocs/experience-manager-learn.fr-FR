---
title: Personnalisation à l’aide du compositeur d’expérience visuelle Adobe Target
description: Tutoriel complet montrant comment créer et diffuser une expérience personnalisée à l’aide du compositeur d’expérience visuelle (VEC) Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 147
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 100%

---

# Personnalisation à l’aide du compositeur d’expérience visuelle

Dans ce chapitre, nous allons créer des expériences à l’aide du **Compositeur d’expérience visuelle** en en effectuant un glisser-déposer, en permutant ou en modifiant la disposition et le contenu d’une page web depuis Target.

## Vue d’ensemble du scénario

La page d’accueil du site WKND présente les activités locales ou les meilleures choses à faire dans une ville sous la forme de disposition de cartes. En tant que personne chargée du marketing, vous avez pour tâche de modifier la page d’accueil en réorganisant les dispositions des cartes pour voir comment elles affectent l’interaction client et déclenchent la conversion.

### Utilisateurs et utilisatrices impliqués

Pour cet exercice, les utilisateurs et utilisatrices suivants doivent être impliqués et effectuer certaines tâches nécessitant un accès administratif.

* **Producteur ou productrice de contenu/créateur ou créatrice de contenu** (Adobe Experience Manager)
* **Marketeur ou marketeuse** (Adobe Target/équipe d’optimisation)

### Page d’accueil du site WKND

![Scénario 1 AEM Target.](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Conditions préalables

* **AEM**
   * [Instance de publication AEM](./implementation.md#getting-aem) exécutée sur le port 4503
   * [AEM intégré à Adobe Target à l’aide d’Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accéder à vos organisations Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fourni avec [Adobe Target](https://experiencecloud.adobe.com)

## Activités de la personne spécialiste marketing

1. La personne chargée du marketing crée une activité de ciblage A/B dans Adobe Target.
   1. Dans la fenêtre Adobe Target, accédez à l’onglet **Activités**.
   2. Cliquez sur le bouton **Créer une activité** et sélectionnez le type d’activité **Test A/B**.
      ![Adobe Target - Créer une activité.](assets/personalization-use-case-2/create-ab-activity.png)
   3. Sélectionnez le canal **Web** et choisissez le **Compositeur d’expérience visuelle**.
   4. Saisissez l’**URL de l’activité** et cliquez sur **Suivant** pour ouvrir le compositeur d’expérience visuelle.
      ![Adobe Target - Créer une activité.](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Pour que le **Compositeur d’expérience visuelle** charge, activez **Autoriser le chargement de scripts non sécurisés** dans votre navigateur et rechargez la page.
      ![Activité de ciblage d’expérience.](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Notez que la page d’accueil du site WKND s’ouvre dans l’éditeur du compositeur d’expérience visuelle.
      ![Compositeur d’expérience visuelle.](assets/personalization-use-case-2/vec.png)
   7. L’**expérience A** fournit la page d’accueil WKND par défaut ; modifions la disposition du contenu pour l’**expérience B**.
      ![Expérience B.](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Cliquez sur l’un des conteneurs de mise en page de carte (*« Meilleurs torréfacteurs »*) et sélectionnez l’option **Réorganiser**.
      ![Sélection de conteneur.](assets/personalization-use-case-3/container-selection.png)
   9. Cliquez sur le conteneur que vous souhaitez réorganiser et faites-le glisser jusqu’à l’emplacement souhaité. Faisons passer le conteneur *« Meilleurs torréfacteurs »* de la 1ère ligne de la 1ère colonne à la 3ème ligne de la 1ère colonne. Le conteneur *« Meilleurs torréfacteurs »* est maintenant à côté du conteneur *« Expositions de photos »*.
      ![Échange de conteneur.](assets/personalization-use-case-3/container-swap.png)
      **Après l’échange**
      ![Conteneur échangé.](assets/personalization-use-case-3/after-swap-1-3.png)
   10. De même, réorganisez les positions des autres conteneurs de carte.
      ![Conteneur échangé.](assets/personalization-use-case-3/after-swap-all.png)
   11. Ajoutons également un texte d’en-tête sous le composant Carrousel et au-dessus de la disposition de carte.
   12. Cliquez sur le conteneur Carrousel et sélectionnez l’option **Insérer après > HTML** pour ajouter un HTML.
      ![Ajout de texte.](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Ajout de texte.](assets/personalization-use-case-3/after-changes.png)
   13. Cliquez sur **Suivant** pour poursuivre votre activité.
   14. Définissez la **Méthode d’affectation du trafic** sur Manuelle et affectez 100 % du trafic vers l’**expérience B**.
      ![Trafic de l’expérience B.](assets/personalization-use-case-2/traffic.png)
   15. Cliquez sur **Suivant**.
   16. Fournissez des **Mesures d’objectif** pour votre activité, puis enregistrez et fermez votre test A/B.
      ![Mesure de l’objectif de test A/B.](assets/personalization-use-case-2/goal-metric.png)
   17. Attribuez un nom (**Actualisation de la page d’accueil WKND**) à votre activité et enregistrez vos modifications.
   18. Dans l’écran Détails de l’activité, veillez à **activer** votre activité.
      ![Activation de l’activité.](assets/personalization-use-case-3/save-activity.png)
   19. Accédez à la page d’accueil WKND (http://localhost:4503/content/wknd/en.html) et notez les modifications que nous avons ajoutées à l’activité de test A/B d’actualisation de la page d’accueil WKND.
      ![Page d’accueil WKND actualisée.](assets/personalization-use-case-3/activity-result.png)
   20. Ouvrez la console de votre navigateur et examinez l’onglet de réseau pour rechercher la réponse cible pour l’activité de test A/B d’actualisation de la page d’accueil WKND.
      ![Activité du réseau.](assets/personalization-use-case-3/activity-result.png)

## Résumé

Dans ce chapitre, une personne chargée du marketing a pu créer une expérience à l’aide du compositeur d’expérience visuelle en faisant glisser et en déposant du contenu, mais aussi en modifiant la disposition d’une page web sans changer le code pour exécuter un test.
