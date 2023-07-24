---
title: Tutoriel Intégration d’Analytics à Commerce
description: Découvrez comment intégrer Analytics à Commerce.
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="Intégration" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 1%

---


# Intégration d’Analytics à Commerce

* Vérifiez que le compte a accès à Adobe Analytics.

* Créez un projet dans Adobe Analytics.

* Créez un schéma.
   * Vous aurez besoin de cette option pour effectuer une sélection parmi les options des étapes suivantes. Pour créer un schéma, consultez la colonne de gauche sous &quot;Data Management&quot; et recherchez les schémas. Maintenant, en haut à gauche, cliquez sur &quot;Créer un schéma&quot;. Sélectionnez XDM ExperienceEvent.
   * Dans le volet de recherche de gauche, cliquez sur Ajouter .
      * Dans la recherche, vous pouvez filtrer en saisissant `ExperienceEvent Commerce`
      * Rechercher `Adobe Analytics ExperienceEvent Commerce` et cochez la case
      * Veillez à cliquer sur le `Add field groups` en haut à droite pour enregistrer et continuer
* Créez un jeu de données. Vous en aurez besoin lors de la configuration suivante de &quot;DataStream&quot;.
   * Le jeu de données se trouve sous la colonne de gauche &quot;Data Management&quot; et recherche &quot;Jeux de données&quot;.
   * Cliquez ensuite sur &quot;Créer un jeu de données&quot; en haut à droite. Vous créez le jeu de données à partir du schéma.
   * rechercher et utiliser le schéma que vous avez créé précédemment ;
* Créer un flux de données. Vous pouvez y accéder en utilisant &quot;Collecte de données dans la colonne de gauche&quot; et en recherchant &quot;Enchaînement de données&quot;.
* Créez des tableaux avec des panneaux et des segments. Cette méthode est très complexe pour ce tutoriel. Vous avez besoin d’une personne Analytics expérimentée pour vous aider.


Enfin, pour afficher votre rapport, accédez à experience.adobe.com pour trouver votre projet d’espace de travail, cliquez sur le lien du projet que vous souhaitez afficher et vous devriez voir quelque chose comme cette image.

![Capture d’écran d’Analytics de certaines données commerciales](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)