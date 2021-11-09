---
title: Navigation dans les panneaux imbriqués
description: Navigation dans les panneaux imbriqués
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
source-git-commit: 84a0c78f89f78e161b460574b5927fc4aba2fe3a
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# Onglets de navigation avec plusieurs panneaux

Lorsque votre formulaire comporte des onglets de navigation de gauche et que l’un des onglets comporte plusieurs panneaux, vous pouvez masquer le titre des panneaux enfants tout en pouvant naviguer entre les onglets et les panneaux enfants de ces onglets.

[Cette fonctionnalité est en ligne sous cette forme.](https://forms.enablementadobe.com/content/forms/af/testnav1.html)




## Créer un formulaire adaptatif

Créez un formulaire adaptatif avec la structure suivante. Le panneau racine contient des panneaux enfants qui s’affichent sous forme d’onglets sur la gauche. Certaines de ces **onglets**&quot; possèdent des panneaux enfants supplémentaires. Par exemple, l’onglet Famille comporte deux panneaux enfants appelés Épouse et Enfants.
Une barre d’outils est également ajoutée sous FormContainer avec les boutons Préc et Suivant .

![toolbar-spacing](assets/multiple-panels.png)



Le comportement par défaut de ce formulaire consiste à afficher tous les panneaux à gauche, puis à naviguer d’un onglet à l’autre en cliquant sur le bouton suivant.

Pour modifier ce comportement par défaut, procédez comme suit :

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


Ajoutez le code suivant à l’événement click de la variable **Suivant** à l’aide de l’éditeur de code

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Ajoutez le code suivant à l’événement click de la variable **Prev** à l’aide de l’éditeur de code

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

Le code ci-dessus vous aidera à naviguer entre les onglets et les panneaux enfants de chaque onglet.

## Masquer le titre des panneaux enfants

Utilisez l’éditeur de style pour masquer le titre des panneaux enfants des onglets.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
> La fonctionnalité décrite dans cet article ne fonctionne pas dans le dernier onglet. Par exemple, si l’onglet Adresse comporte des panneaux enfants, cette fonctionnalité ne fonctionnera pas.