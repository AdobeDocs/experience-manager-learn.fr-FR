---
title: Utilisation du système de style dans AEM Forms
description: Définition de la stratégie de style pour le composant Bouton
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 10%

---

# Définition du style de la stratégie pour le composant

* Connectez-vous à votre instance AEM locale prête pour le cloud et accédez à Outils . | Général | Modèles | nom du projet.

* Sélectionnez et ouvrez le modèle **Vierge avec composants principaux** en mode d’édition.
* Cliquez sur l’icône de stratégie du composant de bouton pour ouvrir l’éditeur de stratégies.

* ![button-policy](assets/button-policy.png)

Définissez la stratégie comme illustré ci-dessous.

![button-policy-details](assets/styling-policy.png)

Nous avons défini deux styles/variations appelés Marketing et Entreprise. Ces variations sont associées aux classes CSS correspondantes.**Assurez-vous qu’il n’y a pas d’espace avant et après les noms de classe CSS**.
Enregistrez vos modifications.

| Style | Classe CSS |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button—marketing |
| Entreprise | cmp-adaptiveform-button—Entreprise |

Ces classes CSS seront définies dans le fichier scss du composant (_button.scss).

## Étapes suivantes

[Définition des classes CSS](./create-variations.md)
