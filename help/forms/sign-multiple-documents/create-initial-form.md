---
title: Créer le formulaire initial pour déclencher le processus
description: Créez un formulaire initial pour déclencher la notification par courrier électronique pour début du processus de signature.
feature: formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 11%

---


# Créer un formulaire initial

Le formulaire initial (Formulaire de refinancement) est utilisé pour signer plusieurs formulaires en déclenchant le flux de travail AEM **Signature de plusieurs Forms**. Vous pouvez saisir les valeurs de votre choix mais vous assurer que les champs suivants sont ajoutés au formulaire.



| Type de champ | Nom | Objectif | Masqué | Valeur par défaut |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | Pour indiquer l’état de la signature | O | N |
| TextField | guid | Pour identifier un formulaire de manière unique | O | 3 889 |
| TextField | customerName | Pour capturer le nom des clients | N |
| TextField | customerEmail | Envoi d’une notification par courrier électronique au client | N |
| Case à cocher | formsToSign | Les éléments identifient les formulaires du package. | N |



Le formulaire initial doit être configuré pour déclencher un processus AEM appelé **signpleforms**
Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Cela est très important car l’exemple de code recherche un fichier appelé Data.xml dans la charge utile du processus d’envoi du formulaire.

## Assets

Le formulaire initial (formulaire de refinancement) peut être [téléchargé ici](assets/refinance-form.zip)





