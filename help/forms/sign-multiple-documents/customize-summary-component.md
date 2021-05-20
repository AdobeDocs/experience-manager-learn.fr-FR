---
title: Personnalisation du composant Résumé
description: Étendez le composant d’étape de résumé pour inclure la possibilité d’accéder au formulaire suivant dans le module.
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

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

summary.jsp comporte le code suivant. Il fait référence à la bibliothèque cliente identifiée par l’ID de catégorie **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

Le composant de résumé personnalisé peut être [téléchargé ici](assets/custom-summary-step.zip)


