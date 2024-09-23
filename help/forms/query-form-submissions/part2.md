---
title: Interroger les envois de formulaire
description: Tutoriel en plusieurs parties détaillant les étapes nécessaires pour interroger les envois de formulaire stockés dans Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '164'
ht-degree: 100%

---

# Créer un envoi personnalisé

Un gestionnaire d’envoi personnalisé a été écrit pour gérer l’envoi du formulaire. À un niveau élevé, le gestionnaire d’envoi personnalisé effectue les opérations suivantes :

* Extrait le nom du formulaire envoyé.
* Extrait les données envoyées. Les données envoyées d’un formulaire basé sur des composants principaux sont toujours au format JSON.
* Extrait et stocke les pièces jointes de formulaire dans le portail Azure. Met à jour les données json envoyées avec l’URL de la pièce jointe.
* Crée des balises d’index blob : recherche la liste des champs pouvant faire l’objet d’une recherche pour le formulaire et sa valeur correspondante à partir des données envoyées.
* Associe les balises d’index blob aux données envoyées et les stocke dans le portail Azure.

La capture d’écran suivante vous montre les balises d’index de blob dans le portail Azure.

![blob-index-tags](assets/blob-index-tags.png)

Le code d’envoi personnalisé se trouve dans **_StoreFormDataWithBlobIndexTagsInAzure_** et le code de stockage et de récupération des données d’Azure figure dans le composant **_SaveAndFetchFromAzure_**.

## Étapes suivantes

[Créer une interface de requête](./part3.md)
