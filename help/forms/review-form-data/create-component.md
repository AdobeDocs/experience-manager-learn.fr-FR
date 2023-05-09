---
title: Création d’un composant pour répertorier les données de formulaire
description: Tutoriel sur la création d’un composant de résumé pour la révision des données de formulaire avant envoi.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# Créer un composant pour résumer les données de formulaire

Un composant simple a été créé pour répertorier les données de formulaire à des fins de révision. Le [fonction de visite de l’API guidebridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) a été utilisé pour effectuer une itération dans les champs de formulaire. Le code de la bibliothèque cliente associé à ce composant reçoit les composants de panneau/tableau du formulaire. Les éléments enfants de ces composants de panneau/tableau du titre des champs de formulaire, la valeur et l’expression SOM sont extraits à l’aide des méthodes de l’API GuidBridge. Un tableau de HTML simple est alors créé avec le titre, la valeur et l’expression SOM pour que l’utilisateur final puisse vérifier/modifier les données du formulaire avant d’envoyer le formulaire.

Par exemple, la capture d’écran ci-dessous montre le tableau créé pour répertorier les champs et leurs valeurs de la variable **YourDetails**. Le dernier TD dans le TR est utilisé pour modifier la valeur du champ à l’aide de l’expression SOM du champ.

![visit-func](assets/visit-function.png)

## Étapes suivantes

[Test de la solution sur votre système local](./deploy-on-your-system.md)
