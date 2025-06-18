---
title: Utilisation du système de style dans AEM Forms
description: Création de variations de style pour le composant de bouton
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: f97a9aed-99d3-4cce-bc3b-c80638e4f1cb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '231'
ht-degree: 100%

---

# Présentation

Le système de style d’Adobe Experience Manager (AEM) permet aux utilisateurs et utilisatrices de créer plusieurs variations visuelles d’un composant, puis de sélectionner le style à utiliser lors de la création d’un formulaire. Cela rend les composants plus flexibles et réutilisables, sans avoir à créer des composants personnalisés pour chaque style.

Cet article vous aidera à créer des variations du composant de bouton et à tester les variations dans votre environnement compatible avec le cloud local avant d’envoyer les modifications à l’instance cloud à l’aide de Cloud Manager.

La capture d’écran affiche les 2 variations de style du composant de bouton disponibles pour l’auteur ou l’autrice du formulaire.


![button-variations](assets/button-variations.png)

## Conditions préalables

* Instance AEM Forms compatible avec le cloud avec les composants principaux.
* Clonage d’un thème : vous devez vous familiariser avec le clonage d’un thème. Pour les besoins de ce tutoriel, nous avons cloné le [thème Chevalet](https://github.com/adobe/aem-forms-theme-easel). Vous pouvez cloner n’importe quel thème disponible en fonction de vos besoins.

* Installez la dernière version d’Apache Maven. Apache Maven est un outil d’automatisation de création couramment utilisé dans les projets Java™. L’installation de la dernière version vous garantit les dépendances nécessaires à la personnalisation du thème.
* Installez un éditeur de texte brut. Par exemple, Microsoft® Visual Studio Code. L’utilisation d’un éditeur de texte brut tel que Microsoft® Visual Studio Code fournit un environnement convivial pour la création et la modification de fichiers de thème.



## Étapes suivantes

[Création d’une politique de style](./style-policy.md)
