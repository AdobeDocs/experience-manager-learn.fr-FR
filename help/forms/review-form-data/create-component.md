---
title: Créer un composant pour répertorier les données de formulaire
description: Tutoriel sur la création d’un composant de résumé pour la révision des données de formulaire avant envoi.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 40
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 100%

---

# Créer un composant pour résumer les données de formulaire

Un composant simple a été créé pour répertorier les données de formulaire à des fins de révision. La [fonction de visite de l’API GuideBridge](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) a été utilisée pour effectuer une itération dans les champs de formulaire. Le code de la bibliothèque cliente associé à ce composant reçoit les composants de panneau/tableau sur le formulaire. Les champs de formulaire titre, la valeur et l’expression SOM sont extraits des éléments enfants de ces composants panneau/tableau à l’aide des méthodes de l’API GuidBridge. Un tableau HTML simple est alors créé avec le titre, la valeur et l’expression SOM pour que la personne finale puisse vérifier/modifier les données du formulaire avant d’envoyer le formulaire.

Par exemple, la copie d’écran ci-dessous montre le tableau créé pour répertorier les champs et leurs valeurs de **YourDetails**. Le dernier élément TD dans l’élément TR est utilisé pour modifier la valeur du champ à l’aide de l’expression SOM du champ.

![visit-func](assets/visit-function.png)

## Étapes suivantes

[Tester la solution sur votre système local](./deploy-on-your-system.md)
