---
title: Utilisation des onglets verticaux dans AEM Forms Cloud Service
description: Création d’un formulaire adaptatif à l’aide d’onglets verticaux
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 5%

---

# Navigation entre les onglets

Vous pouvez naviguer entre les onglets en cliquant sur les onglets individuels ou en utilisant les boutons précédent et suivant du formulaire.
Pour naviguer à l’aide des boutons, ajoutez deux boutons à votre formulaire et nommez-les Précédent et Suivant. Associez la fonction personnalisée suivante à l’événement click du bouton pour naviguer entre les onglets.

Voici la fonction personnalisée pour naviguer entre les onglets.



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

Voici l’éditeur de règles pour les boutons Suivant et Précédent

**Bouton Suivant**

![next-button](assets/next-button.png)

**Bouton précédent**

![prev-button](assets/prev-button.png)
