---
title: Personnaliser le composant Résumé
description: Etendez le composant de l’étape de résumé pour inclure la possibilité de naviguer jusqu’au formulaire suivant dans le package.
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 2%

---


# Personnaliser l&#39;étape de résumé

Le composant d’étape de résumé permet d’afficher le résumé de l’envoi du formulaire avec un lien pour télécharger le formulaire signé. L’étape de résumé est généralement placée dans le dernier panneau du formulaire.
Dans ce cas d&#39;utilisation, nous avons créé un nouveau composant basé sur le composant Résumé prêt à l&#39;emploi et étendu la capacité à inclure clientlib personnalisé.

Ce composant est identifié par l’étiquette Signer un formulaire multiple

La capture d’écran suivante montre le nouveau composant créé pour afficher le message à la fin de la cérémonie de signature.

![composant récapitulatif](assets/summary.PNG)

Le nouveau composant est basé sur le composant de résumé prêt à l’emploi.
![component-prop](assets/componentprop.PNG)

Nous avons ajouté un bouton pour accéder au formulaire suivant en vue de la signature.
![template-code](assets/template-code.PNG)

Le fichier summary.jsp contient le code suivant. Il fait référence à la bibliothèque cliente identifiée par l’id de catégorie **getnextform**.

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


