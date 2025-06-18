---
title: Créer une interface de requête
description: Tutoriel en plusieurs parties détaillant les étapes nécessaires pour interroger les envois de formulaire stockés dans Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 570a8176-ecf8-4a57-ab58-97190214c53f
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '130'
ht-degree: 100%

---

# Créer une interface de requête

Une interface de requête simple a été créée pour permettre à l’« administrateur ou administratrice » de saisir des critères de recherche afin de récupérer des envois de formulaire spécifiques. Les résultats s’affichent alors dans un format tabulaire simple, avec la possibilité d’afficher un envoi de formulaire particulier.

![query-submissions](assets/query-submissions.png)

Les listes déroulantes de cette interface sont des listes déroulantes en cascade. Les options disponibles dans la liste déroulante changent en fonction des sélections effectuées dans la liste déroulante précédente.

Les listes déroulantes sont renseignées à l’aide de sources de données RESTful.

Les résultats de la recherche s’affichent dans un composant personnalisé appelé « SearchResults ». Lorsque la personne clique sur le bouton d’affichage, le formulaire est prérempli avec les données envoyées et les pièces jointes.

## Étapes suivantes

[Écrire le service de préremplissage](./part4.md)
