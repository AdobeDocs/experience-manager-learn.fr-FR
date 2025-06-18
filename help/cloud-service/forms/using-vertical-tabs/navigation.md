---
title: Utilisation des onglets verticaux dans AEM Forms as a Cloud Service
description: Création d’un formulaire adaptatif à l’aide des onglets verticaux
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '111'
ht-degree: 100%

---

# Navigation entre les onglets

Vous pouvez naviguer entre les onglets en cliquant sur chaque onglet ou en utilisant les boutons précédent et suivant du formulaire.
Pour naviguer à l’aide des boutons, ajoutez deux boutons à votre formulaire et attribuez-leur les noms Précédent et Suivant. Associez la fonction personnalisée suivante à l’événement clic du bouton pour naviguer entre les onglets.

Vous trouverez ci-dessous la fonction personnalisée pour naviguer entre les onglets.



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

Vous trouverez ci-dessous l’éditeur de règles pour les boutons Suivant et Précédent

**Bouton Suivant**

![next-button](assets/next-button.png)

**Bouton Précédent**

![prev-button](assets/prev-button.png)
