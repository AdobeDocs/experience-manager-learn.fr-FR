---
title: Utiliser des fonctions et l’éditeur de code
description: Utiliser des fonctions et l’éditeur de code pour créer des règles commerciales
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-4270
thumbnail: 22282.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 7b2a4075-bfdf-49f3-b507-34d86193bf64
duration: 466
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '140'
ht-degree: 100%

---

# Utiliser des fonctions personnalisées et l’éditeur de code {#using-functions-and-code-editor}

Dans cette partie, nous allons utiliser les fonctions personnalisées et l’éditeur de code pour créer des règles commerciales.

Vous avez déjà installé la [bibliothèque cliente avec fonction personnalisée](assets/client-libs-and-logo.zip) plus tôt dans ce tutoriel.

En règle générale, une bibliothèque cliente se compose d’un fichier CSS et JavaScript. Cette bibliothèque cliente contient un fichier JavaScript qui expose une fonction permettant de remplir les valeurs de la liste déroulante.


## Fonction pour remplir la liste déroulante {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=12&learn=on)

### Définir le titre de résumé du panneau {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=12&learn=on)

#### Valider le panneau {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=12&learn=on)

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

Vous pouvez annuler la mise en commentaire de la ligne 1 pour déboguer le code dans la fenêtre du navigateur.

Ligne 4 - Obtention du panneau actuel.

Ligne 5 - Validation du panneau actuel.

Ligne 9 - Si aucune erreur n’est détectée, passez au panneau suivant.

Prévisualisez le formulaire et testez la nouvelle fonctionnalité activée.
