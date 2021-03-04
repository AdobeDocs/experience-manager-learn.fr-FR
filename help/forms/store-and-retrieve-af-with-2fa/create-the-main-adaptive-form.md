---
title: Créer le formulaire adaptatif principal
description: Créez des formulaires adaptatifs pour capturer les informations sur le demandeur et le formulaire adaptatif pour récupérer le formulaire adaptatif enregistré.
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Développement
role: Professionnel
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 1%

---


# Créer le formulaire adaptatif principal

Le formulaire **StoreAFWithAttachments** est le formulaire adaptatif principal. Ce formulaire adaptatif est le point d’entrée du cas d’utilisation. Dans ce formulaire, les détails de l’utilisateur, y compris le numéro de mobile, sont capturés. Ce formulaire peut également ajouter des pièces jointes. Lorsque l’utilisateur clique sur le bouton Enregistrer et quitter, le code côté serveur est exécuté pour stocker les données du formulaire dans la base de données et un identifiant d’application unique est généré et présenté à l’utilisateur pour une conservation sécurisée. Cet ID d&#39;application sera utilisé pour récupérer le numéro de mobile associé à l&#39;application.

![formulaire de demande principal](assets/6552.JPG)

Ce formulaire est associé aux bibliothèques client **bootboxjs540,storeAFWithAttachments** créées plus tôt dans le cours et à un processus AEM qui est déclenché lors de l’envoi du formulaire.


* Les exemples de formulaires sont basés sur [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* Le [formulaire StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) terminé peut être téléchargé et importé dans votre instance AEM.

* Le processus [AEM associé à ce formulaire](assets/workflow-model-store-af-with-attachments.zip) doit être importé dans votre instance AEM pour que le formulaire fonctionne.



