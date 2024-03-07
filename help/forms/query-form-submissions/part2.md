---
title: Requête sur l’envoi du formulaire
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
source-wordcount: '164'
ht-degree: 3%

---

# Créer un envoi personnalisé

Un gestionnaire d’envoi personnalisé a été créé pour gérer l’envoi du formulaire. À un niveau élevé, le gestionnaire d’envoi personnalisé effectue les opérations suivantes :

* Extrait le nom du formulaire envoyé.
* Extrait les données envoyées. Les données envoyées d’un formulaire basé sur un composant principal sont toujours au format JSON.
* Extrait et stocke les pièces jointes de formulaire dans le portail Azure. Met à jour les données json envoyées avec l’URL de la pièce jointe.
* Crée des balises d’index blob : recherche la liste des champs pouvant faire l’objet d’une recherche pour le formulaire et sa valeur correspondante à partir des données envoyées.
* Associe les balises d’index blob aux données envoyées et les stocke dans le portail Azure.

La capture d’écran suivante vous montre les balises d’index de blob dans le portail Azure

![blob-index-tags](assets/blob-index-tags.png)

Le code d’envoi personnalisé se trouve dans **_StoreFormDataWithBlobIndexTagsInAzure_** et le code de stockage et de récupération des données d’Azure se trouve dans le composant . **_SaveAndFetchFromAzure_**

## Étapes suivantes

[Interface de requête de création](./part3.md)

