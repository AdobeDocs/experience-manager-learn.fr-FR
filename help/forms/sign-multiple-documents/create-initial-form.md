---
title: Création du formulaire initial pour déclencher le processus
description: Créez un formulaire initial pour déclencher la notification par courrier électronique pour lancer le processus de signature.
feature: Formulaires adaptatifs
version: 6.4,6.5
topic: Développement
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 13%

---


# Créer un formulaire initial

Le formulaire initial (Refinance Form) est utilisé pour signer plusieurs formulaires en déclenchant le processus d’AEM **Sign Multiple Forms**. Vous pouvez saisir les valeurs de votre choix mais vous assurer que les champs suivants sont ajoutés au formulaire.

| Type de champ | Nom | Objectif | Masqué  | Valeur par défaut |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| TextField | signed | Pour indiquer l’état de signature | O | N |
| TextField | guid | Pour identifier un formulaire de manière unique | O | 3889 |
| TextField | customerName | Pour capturer le nom des clients | N |
| TextField | customerEmail | Email du client pour envoyer une notification | N |
| Case à cocher | formsToSign | Les éléments identifient les formulaires dans le module. | N |

Le formulaire initial doit être configuré pour déclencher un processus AEM appelé **signmultipleforms**
Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Ceci est très important, car l’exemple de code recherche un fichier appelé Data.xml dans la payload du processus d’envoi du formulaire.

## Assets

Le formulaire initial (formulaire de refinancement) peut être [téléchargé ici](assets/refinance-form.zip)





