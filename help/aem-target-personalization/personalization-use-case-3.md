---
title: Personnalisation à l’aide du compositeur d’expérience visuelle Adobe Target
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Tutoriel complet montrant comment créer et diffuser une expérience personnalisée à l’aide du compositeur d’expérience visuelle (VEC) d’Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 2%

---

# Personnalisation à l’aide du compositeur d’expérience visuelle

Dans ce chapitre, nous allons explorer la création d’expériences à l’aide de **Compositeur d’expérience visuelle** en faisant glisser, en permutant et en modifiant la mise en page et le contenu d’une page web depuis Target.

## Présentation du scénario

La page d’accueil du site WKND présente les activités locales ou les meilleures choses à faire autour d’une ville sous la forme de mises en page de cartes. En tant que marketeur, vous avez reçu la tâche de modifier la page d’accueil, en réorganisant les mises en page des cartes pour voir comment elles affectent l’engagement des utilisateurs et déclenchent la conversion.

### Utilisateurs impliqués

Pour cet exercice, les utilisateurs suivants doivent être impliqués et effectuer certaines tâches nécessitant un accès administratif.

* **Producteur de contenu/éditeur de contenu** (Adobe Experience Manager)
* **Marketer** (Adobe Target/équipe d’optimisation)

### Page d’accueil du site WKND

![Scénario 1 AEM](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Prérequis

* **AEM **
   * [AEM instance de publication](./implementation.md#getting-aem) s’exécutant sur 4503
   * [AEM intégré à Adobe Target à l’aide d’Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Accès à vos organisations Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud fourni avec [Adobe Target](https://experiencecloud.adobe.com)

## Activités marketing

1. Le marketeur crée une activité de ciblage A/B dans Adobe Target.
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
   7. **Expérience A** fournit la page d’accueil WKND par défaut et modifions la disposition du contenu pour **Expérience B**.
      ![Expérience B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Cliquez sur l’un des conteneurs de mise en page de carte (*Meilleurs rotors*) et sélectionnez **Réorganiser** .
      ![Sélection du conteneur](assets/personalization-use-case-3/container-selection.png)
   9. Cliquez sur le conteneur que vous souhaitez réorganiser et faites-le glisser jusqu’à l’emplacement souhaité. Réorganisons le *Meilleurs rotors* conteneur de la 1ère ligne de la 1ère colonne à la 3e ligne de la 1ère colonne. Maintenant, la *Meilleurs rotors* conteneur est à côté de *Expositions de photos* conteneur.
      ![Permutation de conteneur](assets/personalization-use-case-3/container-swap.png)

      **Après le changement**
      ![Conteneur échangé](assets/personalization-use-case-3/after-swap-1-3.png)
   10. De même, réorganisez les positions des autres conteneurs de carte.
      ![Conteneur échangé](assets/personalization-use-case-3/after-swap-all.png)
   11. Ajoutons également un texte d’en-tête sous le composant du carrousel et au-dessus de la disposition de la carte.
   12. Cliquez sur le conteneur de carrousel et sélectionnez l’option **Insérer après > HTML** pour ajouter un HTML.
      ![Ajouter du texte](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Ajouter du texte](assets/personalization-use-case-3/after-changes.png)
   13. Cliquez sur **Suivant** pour poursuivre votre activité.
   14. Sélectionnez la **Méthode d’affectation du trafic** comme manuel et affecter 100 % du trafic à **Expérience B**.
      ![Trafic de l’expérience B](assets/personalization-use-case-2/traffic.png)
   15. Cliquez sur **Suivant**.
   16. Fournir **Mesures d’objectif** pour votre activité et enregistrez et fermez votre test A/B.
      ![Mesure de l’objectif de test A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Attribuez un nom (**Actualisation de la page d’accueil WKND**) pour votre activité et enregistrez vos modifications.
   18. Dans l’écran Détails de l’activité, veillez à **Activer** votre activité.
      ![Activer l’activité](assets/personalization-use-case-3/save-activity.png)
   19. Accédez à la page d’accueil WKND (http://localhost:4503/content/wknd/en.html) et notez les modifications que nous avons ajoutées à l’activité de test A/B d’actualisation de la page d’accueil WKND .
      ![Actualisation de la page d’accueil WKND](assets/personalization-use-case-3/activity-result.png)
   20. Ouvrez la console de votre navigateur et examinez l’onglet réseau pour rechercher la réponse cible pour l’activité de test A/B Actualisation de la page d’accueil WKND .
      ![Activité du réseau](assets/personalization-use-case-3/activity-result.png)

## Résumé

Dans ce chapitre, un marketeur a pu créer une expérience à l’aide du compositeur d’expérience visuelle en faisant glisser et en déposant, en permutant et en modifiant la mise en page et le contenu d’une page web sans modifier de code pour exécuter un test.
