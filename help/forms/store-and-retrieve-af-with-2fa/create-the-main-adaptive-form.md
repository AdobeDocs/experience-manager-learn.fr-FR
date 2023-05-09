---
title: Créer le formulaire adaptatif principal
description: Créer des formulaires adaptatifs pour capturer les informations sur le demandeur et le formulaire adaptatif pour récupérer le formulaire adaptatif enregistré
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Créer le formulaire adaptatif principal

Le formulaire **StoreAFWithAttachments** est le formulaire adaptatif principal. Ce formulaire adaptatif est le point d’entrée du cas d’utilisation. Dans ce formulaire, les détails de l’utilisateur, y compris le numéro de mobile, sont capturés. Ce formulaire peut également ajouter des pièces jointes. Lorsque l’utilisateur clique sur le bouton Enregistrer et quitter , le code côté serveur est exécuté pour stocker les données de formulaire dans la base de données. Un identifiant d’application unique est généré et présenté à l’utilisateur afin d’être sécurisé. Cet identifiant d’application est utilisé pour récupérer le numéro de mobile associé à l’application.

![formulaire d&#39;application principal](assets/6552.JPG)

Ce formulaire est associé à **bootboxjs540,storeAFWithAttachments** bibliothèques clientes créées plus tôt dans le cours et un workflow d’AEM qui est déclenché lors de l’envoi du formulaire.


* Les exemples de formulaires reposent sur [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* Terminé [Formulaire StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) peut être téléchargé et importé dans votre instance AEM.

* Le [Processus AEM associé à ce formulaire](assets/workflow-model-store-af-with-attachments.zip) doit être importé dans votre instance AEM pour que le formulaire fonctionne.


## Étapes suivantes

[Créer le formulaire récupérant le formulaire enregistré](./retrieve-saved-form.md)