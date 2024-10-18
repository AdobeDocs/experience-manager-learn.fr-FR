---
title: Utilisation du système de style dans AEM Forms
description: Définition de la stratégie de style pour le composant de bouton
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
workflow-type: ht
source-wordcount: '139'
ht-degree: 100%

---

# Définition du style de la politique pour le composant

* Connectez-vous à votre instance AEM locale prête pour le cloud et accédez à Outils | Général | Modèles | Nom de votre projet.

* Sélectionnez et ouvrez le modèle **Vierge avec composants principaux** en mode d’édition.
* Cliquez sur l’icône de politique du composant de bouton pour ouvrir l’éditeur de politique.

* ![button-policy](assets/button-policy.png)

Définissez la politique comme illustré ci-dessous.

![button-policy-details](assets/styling-policy.png)

Nous avons défini 2 styles/variations appelés Marketing et Corporate. Ces variations sont associées aux classes CSS correspondantes.**Assurez-vous qu’il n’y a pas d’espace avant et après les noms de classe CSS**.
Enregistrez vos modifications.

| Style | Classe CSS |
|-----------|------------------------------------|
| Marketing | cmp-adaptiveform-button--marketing |
| Corporate | cmp-adaptiveform-button--corporate |

Ces classes CSS seront définies dans le fichier scss du composant (_button.scss).

## Étapes suivantes

[Définition des classes CSS](./create-variations.md)
