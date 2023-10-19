---
title: Associer le composant de page au nouveau modèle de formulaire adaptatif
description: Créer un composant de page
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 100%

---

# Associer le composant de page au modèle

L’étape suivante consiste à associer le composant de page au nouveau modèle de formulaire adaptatif. Cela garantit que le code du composant de page sera exécuté chaque fois qu’un formulaire adaptatif basé sur le nouveau modèle est rendu. Pour les besoins de ce tutoriel, un nouveau modèle de formulaire adaptatif appelé **StoreAndRestoreFromAzure** a été créé dans le dossier **AzurePortalStorage**.
Accédez au nœud /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content, ajoutez la propriété suivante et enregistrez les modifications.

| **Nom de la propriété** | **Type de propriété** | **Valeur de la propriété** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Chaîne | azureportalpagecomponent/component/page/storeandfetch |

Accédez au nœud /conf/AzurePortalStorage/settings/wcm/templates/storeandrestoreformazure/structure/jcr:content, ajoutez la propriété suivante et enregistrez les modifications.
| **Nom de la propriété** | **Type de propriété** | **Valeur de la propriété** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Chaîne | azureportalpagecomponent/component/page/storeandfetch |


## Étapes suivantes

[Créer une intégration avec le stockage Azure Storage](./create-fdm.md)
