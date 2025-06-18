---
title: Associer le composant de page au nouveau modèle de formulaire adaptatif
description: Créer un composant de page
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '150'
ht-degree: 100%

---

# Associer le composant de page au modèle

L’étape suivante consiste à associer le composant de page au nouveau modèle de formulaire adaptatif. Cela garantit que le code du composant de page sera exécuté chaque fois qu’un formulaire adaptatif basé sur le nouveau modèle est rendu. Pour les besoins de ce tutoriel, un nouveau modèle de formulaire adaptatif appelé **StoreAndRestoreFromAzure** a été créé dans le dossier **AzurePortalStorage**.
Accédez au nœud /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content, ajoutez la propriété suivante et enregistrez les modifications.

| **Nom de la propriété** | **Type de propriété** | **Valeur de la propriété** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Chaîne | azureportalpagecomponent/component/page/storeandfetch |

Accédez au nœud /conf/AzurePortalStorage/settings/wcm/templates/storeandrestoreformazure/structure/jcr:content, ajoutez la propriété ci-après et enregistrez les modifications.

| **Nom de la propriété** | **Type de propriété** | **Valeur de la propriété** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Chaîne | azureportalpagecomponent/component/page/storeandfetch |


## Étapes suivantes

[Créer une intégration avec le stockage Azure Storage](./create-fdm.md)
