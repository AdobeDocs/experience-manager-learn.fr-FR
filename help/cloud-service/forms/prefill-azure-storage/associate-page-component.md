---
title: Associer le composant de page au nouveau modèle de formulaire adaptatif
description: Création d’un composant de page
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 7%

---

# Associer le composant de page au modèle

L’étape suivante consiste à associer le composant de page au nouveau modèle de formulaire adaptatif. Cela garantit que le code du composant de page sera exécuté chaque fois qu’un formulaire adaptatif basé sur le nouveau modèle est rendu. Pour les besoins de ce tutoriel, un nouveau modèle de formulaire adaptatif appelé **StoreAndRestoreFromAzure** a été créé dans le **AzurePortalStorage** dossier.
Accédez au noeud /conf/AzurePortalStorage/settings/wcm/templates/storeandrestoreformazure/initial/jcr:content , ajoutez la propriété suivante et enregistrez les modifications.

| **Nom de la propriété** | **Type de propriété** | **Valeur de la propriété** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Chaîne | azureportalpagecomponent/component/page/storeandfetch |

Accédez au noeud /conf/AzurePortalStorage/settings/wcm/templates/storeandrestoreformazure/structure/jcr:content , ajoutez la propriété suivante et enregistrez les modifications.
| **Nom de la propriété**  | **Type de propriété** | **Valeur de la propriété**                                    | |—|—|—| | sling:resourceType | Chaîne | azureportalpagecomponent/component/page/storeandfetch |


## Étapes suivantes

[Création d’une intégration avec Azure Storage](./create-fdm.md)
