---
title: Interface de requête de création
description: Tutoriel en plusieurs parties pour vous guider à travers les étapes impliquées dans l’interrogation des envois de formulaire stockés dans Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---

# Interface de requête de création

Une interface de requête simple a été créée pour permettre à &quot;l’administrateur&quot; de saisir des critères de recherche afin de récupérer des envois de formulaire spécifiques. Les résultats s’affichent alors dans un format tabulaire simple, avec la possibilité d’afficher un envoi de formulaire particulier.

![query-submission](assets/query-submissions.png)

Les listes déroulantes de cette interface sont des listes déroulantes en cascade. Les options disponibles dans la liste déroulante changent en fonction des sélections effectuées dans la liste déroulante précédente.

Les listes déroulantes sont renseignées à l’aide de sources de données RESTful.

Les résultats de la recherche s’affichent dans un composant personnalisé appelé &quot;SearchResults&quot;. Lorsque l’utilisateur clique sur le bouton d’affichage, le formulaire est prérempli avec les données envoyées et les pièces jointes.

## Étapes suivantes

[Écrire le service de préremplissage](./part4.md)
