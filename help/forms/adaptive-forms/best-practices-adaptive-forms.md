---
title: Conventions de dénomination et bonnes pratiques à suivre lors de la création de formulaires adaptatifs
description: Conventions de dénomination et bonnes pratiques à suivre lors de la création de formulaires adaptatifs
feature: Formulaires adaptatifs
version: 6.3,6.4,6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 8%

---

# Bonnes pratiques

Adobe Experience Manager (AEM) Forms vous permet de transformer des opérations complexes en de simples et remarquables expériences numériques. Le document suivant décrit certaines autres bonnes pratiques à suivre lors du développement d’Adaptive Forms. Ce document est destiné à être utilisé conjointement avec [ce document](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview).

## Conventions de dénomination

* **Panneaux**
   * Les noms de panneau sont des majuscules, en commençant par un caractère majuscule.

* **Champs de formulaire**
   * Les noms de champ sont des majuscules commençant par un caractère minuscule.
   * Ne pas commencer les noms de champ par des nombres
   * N’incluez pas de tirets &quot;-&quot; dans vos noms. Cela équivaut à un signe moins dans votre code et agit en tant qu’opérateurs dans votre code.
   * Les noms peuvent contenir des lettres, des chiffres, des traits de soulignement et des signes dollar.
   * Les noms doivent commencer par une lettre
   * Les noms sont sensibles à la casse
   * Les mots réservés (tels que les mots-clés JavaScript) ne peuvent pas être utilisés comme noms. Attention aux autres mots réservés spécifiques à l&#39;AF tels que   comme &quot;panel&quot;,&quot;name&quot;.
   * N’incluez pas de tirets &quot;-&quot; dans vos noms.
* **Développement de Forms**
   * Les fragments de formulaire doivent être pris en compte lors du développement de formulaires volumineux. Activation du chargement différé des fragments de formulaire pour un chargement plus rapide   times
   * **DataModel**
      * Il est recommandé d’associer un formulaire adaptatif au modèle de données approprié.
   * **Événements d’objet**
      * Le code associé à la visibilité d’un objet doit toujours être placé dans l’événement de visibilité de cet objet.
   * **Script**
      * Si le code que vous écrivez dans un formulaire adaptatif s’étend au-delà de 5 lignes visibles, vous devez déplacer votre code vers une bibliothèque cliente. Dans l’idéal, ajoutez votre fonction à la bibliothèque cliente, puis ajoutez les balises jsdoc appropriées pour que la fonction soit visible dans l’éditeur de règles de formulaire adaptatif.


