---
title: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
seo-title: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
description: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
seo-description: Conventions et bonnes pratiques de nommage à appliquer lors de la création de formulaires adaptatifs
feature: Formulaires adaptatifs
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 8%

---

# Bonnes pratiques

Adobe Experience Manager (AEM) Forms vous permet de transformer des opérations complexes en de simples et remarquables expériences numériques. Le document suivant décrit quelques autres bonnes pratiques à suivre lors du développement d&#39;Adaptive Forms. Ce document est destiné à être utilisé conjointement avec [ce document](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview).

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
   * Les mots réservés (tels que les mots-clés JavaScript) ne peuvent pas être utilisés comme noms. Attention aux autres mots réservés spécifiques à la FA tels que   comme &quot;panel&quot;,&quot;name&quot;.
   * N’incluez pas de tirets &quot;-&quot; dans vos noms.
* **Développement de Forms**
   * Les fragments de formulaire doivent être pris en compte lors du développement de formulaires volumineux. Activer le chargement différé des fragments de formulaire pour un chargement plus rapide   fois
   * **DataModel**
      * Il est recommandé d’associer un formulaire adaptatif au modèle de données approprié.
   * **Événements d’objet**
      * Le code lié à la visibilité d&#39;un objet doit toujours être placé dans le événement de visibilité de cet objet.
   * **Script**
      * Si le code que vous écrivez dans un formulaire adaptatif dépasse 5 lignes visibles, vous devez déplacer votre code vers une bibliothèque cliente. Dans l’idéal, ajoutez votre fonction à la bibliothèque cliente, puis ajoutez les balises jsdoc appropriées pour permettre à la fonction d’être visible dans l’éditeur de règles de formulaire adaptatif.


