---
title: Utilisation des fonctions et de l’éditeur de code
seo-title: Utilisation des fonctions et de l’éditeur de code
description: Utilisation de fonctions et d’un éditeur de code pour créer des règles de fonctionnement
seo-description: Utilisation de fonctions et d’un éditeur de code pour créer des règles de fonctionnement
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: Formulaires adaptatifs
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 1%

---


# Utilisation de fonctions personnalisées et de l’éditeur de code {#using-functions-and-code-editor}

Dans cette partie, nous utiliserons les fonctions personnalisées et l’éditeur de code pour créer des règles de fonctionnement.

vous avez déjà installé [ClientLib avec la fonction personnalisée](assets/client-libs-and-logo.zip) plus tôt dans ce tutoriel.

En règle générale, une bibliothèque cliente se compose d’un fichier CSS et JavaScript. Cette bibliothèque cliente contient un fichier JavaScript qui expose une fonction permettant de renseigner les valeurs de la liste déroulante.


## Fonction à renseigner dans la liste déroulante {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### Définir le titre du résumé du panneau {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### Validation du panneau {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

Voici le code utilisé pour valider les champs du panneau.

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

Vous pouvez annuler la mise en commentaire de la ligne 1 pour déboguer le code dans la fenêtre du navigateur.

Ligne 4 - Obtention du panneau actuel

Ligne 5 - Validez le panneau actuel.

Ligne 9 - Si aucune erreur n’est déplacée vers le panneau suivant

Prévisualisez le formulaire et testez la nouvelle fonctionnalité activée.
