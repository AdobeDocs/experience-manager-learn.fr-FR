---
title: Naviguer dans les panneaux imbriqués
description: Naviguer dans les panneaux imbriqués
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 277
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 100%

---

# Onglets de navigation avec plusieurs panneaux

Lorsque votre formulaire comporte des onglets de navigation de gauche et que l’un des onglets comporte plusieurs panneaux, vous pouvez masquer le titre des panneaux enfants tout en naviguant entre les onglets et les panneaux enfants de cet onglet.

## Créer un formulaire adaptatif

Créez un formulaire adaptatif avec la structure suivante. Le panneau racine contient des panneaux enfants qui s’affichent sous forme d’onglets sur la gauche. Certains de ces « **onglets** » possèdent des panneaux enfants supplémentaires. Par exemple, l’onglet Famille comporte deux panneaux enfants appelés Conjoint ou conjointe et Enfants.

Une barre d’outils est également ajoutée sous conteneur de formulaires avec les boutons Précédent et Suivant.

![toolbar-spacing](assets/multiple-panels.png)



Le comportement par défaut de ce formulaire consiste à afficher tous les panneaux à gauche, puis à naviguer d’un onglet à l’autre en cliquant sur le bouton Suivant.

Pour modifier ce comportement par défaut, procédez comme suit :

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


Ajoutez le code suivant à l’événement clic du bouton **Suivant** à l’aide de l’éditeur de code.

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

Ajoutez le code suivant à l’événement clic du bouton **Précédent** à l’aide de l’éditeur de code.

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

Le code ci-dessus vous aidera à naviguer entre les onglets et les panneaux enfants de chaque onglet.

## Masquer le titre des panneaux enfants

Utilisez l’éditeur de style pour masquer le titre des panneaux enfants des onglets.

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>La fonctionnalité décrite dans cet article ne fonctionne pas dans le dernier onglet. Par exemple, si l’onglet Adresse comporte des panneaux enfants, cette fonctionnalité ne fonctionnera pas.
