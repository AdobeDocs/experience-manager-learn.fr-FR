---
title: Création du formulaire initial pour déclencher le processus
description: Créez un formulaire initial pour déclencher la notification par courrier électronique pour lancer le processus de signature.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 7%

---

# Créer un formulaire initial

Le formulaire initial (formulaire de refinancement) est utilisé pour signer plusieurs formulaires en déclenchant l’événement **Signature de plusieurs Forms** AEM workflow. Vous pouvez saisir les valeurs de votre choix mais vous assurer que les champs suivants sont ajoutés au formulaire.

| Type de champ | Nom | Objectif | Masqué  | Valeur par défaut |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | signed | Pour indiquer l’état de signature | Y | N |
| TextField | guid | Pour identifier un formulaire de manière unique | Y | 3889 |
| TextField | customerName | Pour capturer le nom des clients | N |
| TextField | customerEmail | Email du client pour envoyer une notification | N |
| Case à cocher | formsToSign | Les éléments identifient les formulaires dans le module. | N |

Le formulaire initial doit être configuré pour déclencher un processus AEM appelé **signmultipleforms**
Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Ceci est très important, car l’exemple de code recherche un fichier appelé Data.xml dans la payload du processus d’envoi du formulaire.

## Ressources

Le formulaire initial (formulaire de refinancement) peut être [téléchargé ici](assets/refinance-form.zip)

## Étapes suivantes

[Créer des formulaires à utiliser pour la signature](./create-forms-for-signing.md)
