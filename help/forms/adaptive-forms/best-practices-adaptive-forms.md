---
title: Conventions de nommage et bonnes pratiques à suivre lors de la création de formulaires adaptatifs
description: Conventions de nommage et bonnes pratiques à suivre lors de la création de formulaires adaptatifs
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 65
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 100%

---

# Bonnes pratiques

Les formulaires Adobe Experience Manager (AEM) peuvent vous aider à transformer des transactions complexes en expériences numériques simples et agréables. Le document suivant décrit d’autres bonnes pratiques à suivre lors du développement de formulaires adaptatifs. Utilisez ce document conjointement avec [ce document](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html?lang=fr#Overview).

## Conventions de nommage

* **Panneaux**
   * Les noms des panneaux sont rédigés en camel case et commencent par une lettre majuscule.

* **Champs de formulaire**
   * Les noms des champs sont rédigés en camel case et commencent par une minuscule.
   * Ne commencez pas les noms des champs par un chiffre.
   * N’insérez pas de tirets « - » dans vos noms. Le tiret équivaut à un signe « moins » dans le code et agit en tant qu’opérateur.
   * Les noms peuvent contenir des lettres, des chiffres, des traits de soulignement et des symboles dollar.
   * Les noms doivent commencer par une lettre.
   * Les noms respectent la casse.
   * Les mots réservés (tels que les mots-clés JavaScript) ne peuvent pas être utilisés comme noms. N’incluez pas les mots réservés spécifiques à l’AF tels que « panel » et « name ».
   * N’insérez pas de tirets « - » entre les noms.
* **Développer les formulaires**
   * Les fragments de formulaire doivent être pris en compte lors du développement de formulaires volumineux. Activez le chargement différé des fragments de formulaire afin d’accélérer les temps de chargement.
   * **Modèle de données**
      * Il est recommandé d’associer un formulaire adaptatif au modèle de données approprié.

   * **Événements d’objet**
      * Le code reflétant la visibilité d’un objet doit toujours être placé dans l’événement de visibilité de cet objet.
   * **Script**
      * Si le code que vous écrivez dans un formulaire adaptatif s’étend au-delà de 5 lignes visibles, vous devez déplacer votre code vers une bibliothèque cliente. Dans l’idéal, ajoutez votre fonction à la bibliothèque cliente, puis ajoutez les balises jsdoc appropriées pour que la fonction soit visible dans l’éditeur de règles de formulaire adaptatif.
