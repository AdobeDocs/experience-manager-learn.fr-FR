---
title: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
seo-title: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
description: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
seo-description: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 7%

---

# Bonnes pratiques

Adobe Experience Manager (AEM) Forms vous permet de transformer des opérations complexes en de simples et remarquables expériences numériques. Le document suivant décrit quelques autres bonnes pratiques à suivre lors du développement d&#39;Adaptive Forms. Ce document doit être utilisé conjointement avec [ce document.](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## Conventions de dénomination

* **Panneaux**
   * Les noms de panneau sont des majuscules de chameau commençant par un caractère majuscule.

* **Champs de formulaire**
   * Les noms de champ sont des majuscules de chameau commençant par un caractère minuscule.
   * Ne pas début les noms de champs avec des nombres
   * N’incluez pas de tirets &quot;-&quot; dans vos noms. Cela équivaut à un signe moins dans votre code et agit comme opérateurs dans votre code.
   * Les noms peuvent contenir des lettres, des chiffres, des traits de soulignement et des signes dollar.
   * Les noms doivent commencer par une lettre
   * Les noms respectent la casse
   * Les mots réservés (tels que les mots-clés JavaScript) ne peuvent pas être utilisés comme noms. Attention à d&#39;autres mots réservés spécifiques à AF tels que &quot;panneau&quot;, &quot;nom&quot;.
   * N’incluez pas de tirets &quot;-&quot; dans vos noms.
* **Développement de Forms**
   * Les fragments de formulaire doivent être pris en compte lors du développement de formulaires volumineux. Activer le chargement différé des fragments de formulaire pour accélérer le chargement
   * **DataModel**
      * Il est recommandé d’associer un formulaire adaptatif au modèle de données approprié.
   * **Événements d’objet**
      * Le code lié à la visibilité d&#39;un objet doit toujours être placé dans le événement de visibilité de cet objet.
   * **Script**
      * Si le code que vous écrivez dans un formulaire adaptatif dépasse 5 lignes visibles, vous devez déplacer votre code vers une bibliothèque cliente. Dans l’idéal, ajoutez votre fonction à la bibliothèque cliente, puis ajoutez les balises jsdoc appropriées pour permettre à la fonction d’être visible dans l’éditeur de règles de formulaire adaptatif.


