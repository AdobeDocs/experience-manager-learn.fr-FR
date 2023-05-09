---
title: Personnalisation du composant Résumé
description: Étendez le composant d’étape de résumé pour inclure la possibilité d’accéder au formulaire suivant dans le module.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Personnalisation de l’étape de résumé

Le composant d’étape Résumé est utilisé pour afficher le résumé de votre envoi de formulaire avec un lien pour télécharger le formulaire signé. L’étape Résumé est généralement placée dans le dernier panneau de votre formulaire.
Pour les besoins de ce cas d’utilisation, nous avons créé un nouveau composant basé sur le composant Résumé prêt à l’emploi et étendu la fonctionnalité afin d’inclure la bibliothèque cliente personnalisée.

Ce composant est identifié par le libellé Signer plusieurs formulaires

La capture d’écran suivante montre le nouveau composant créé pour afficher le message à la fin de la cérémonie de signature.

![composant de résumé](assets/summary.PNG)

Le nouveau composant est basé sur le composant de résumé prêt à l’emploi.
![component-prop](assets/componentprop.PNG)

Nous avons ajouté un bouton pour accéder au formulaire suivant à signer.
![template-code](assets/template-code.PNG)

summary.jsp comporte le code suivant. Il fait référence à la bibliothèque cliente identifiée par l’ID de catégorie. **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Ressources

Le composant de résumé personnalisé peut être [téléchargé ici](assets/custom-summary-step.zip)

## Étapes suivantes

[Obtenir le formulaire suivant pour la signature](./create-client-lib.md)