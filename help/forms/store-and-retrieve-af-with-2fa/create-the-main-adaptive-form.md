---
title: Créer le formulaire adaptatif principal
description: Créez des formulaires adaptatifs pour capturer les informations sur le demandeur et le formulaire adaptatif pour récupérer le formulaire adaptatif enregistré.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# Créer le formulaire adaptatif principal

Le formulaire **StoreAFWithAttachments** est le formulaire adaptatif principal. Ce formulaire adaptatif est le point d’entrée du cas d’utilisation. Dans ce formulaire, les détails de l’utilisateur, y compris le numéro de mobile, sont capturés. Ce formulaire peut également ajouter des pièces jointes. Lorsque l’utilisateur clique sur le bouton Enregistrer et quitter, le code côté serveur est exécuté pour stocker les données du formulaire dans la base de données et un identifiant d’application unique est généré et présenté à l’utilisateur pour une conservation sécurisée. Cet ID d&#39;application sera utilisé pour récupérer le numéro de mobile associé à l&#39;application.

![formulaire de demande principal](assets/6552.JPG)

Ce formulaire est associé aux bibliothèques clientes **bootboxjs540,storeAFWithAttachments** créées plus tôt dans le cours et à un processus AEM qui est déclenché lors de l’envoi du formulaire.


* Les exemples de formulaires sont basés sur un modèle [de formulaire adaptatif](assets/custom-template-with-page-component.zip) personnalisé qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* Le formulaire [](assets/store-af-with-attachments-form.zip) StoreAfWithAttachments renseigné peut être téléchargé et importé dans votre instance AEM.

* Le processus [AEM associé à ce formulaire](assets/workflow-model-store-af-with-attachments.zip) doit être importé dans votre instance AEM pour que le formulaire fonctionne.



