---
title: Créer le formulaire adaptatif principal
description: Créer des formulaires adaptatifs pour capturer les informations sur le demandeur et le formulaire adaptatif pour récupérer le formulaire adaptatif enregistré
feature: Formulaires adaptatifs
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Développement
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---


# Créer le formulaire adaptatif principal

Le formulaire **StoreAFWithAttachments** est le formulaire adaptatif principal. Ce formulaire adaptatif est le point d’entrée du cas d’utilisation. Dans ce formulaire, les détails de l’utilisateur, y compris le numéro de mobile, sont capturés. Ce formulaire peut également ajouter des pièces jointes. Lorsque l’utilisateur clique sur le bouton Enregistrer et quitter , le code côté serveur est exécuté pour stocker les données de formulaire dans la base de données. Un identifiant d’application unique est généré et présenté à l’utilisateur afin d’être sécurisé. Cet identifiant d’application sera utilisé pour récupérer le numéro de mobile associé à l’application.

![formulaire d&#39;application principal](assets/6552.JPG)

Ce formulaire est associé aux bibliothèques clientes **bootboxjs540,storeAFWithAttachments** créées plus tôt dans le cours et à un workflow d’AEM qui est déclenché lors de l’envoi du formulaire.


* Les exemples de formulaires sont basés sur [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires soient correctement rendus.

* Le [formulaire StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) terminé peut être téléchargé et importé dans votre instance AEM.

* Le [processus AEM associé à ce formulaire](assets/workflow-model-store-af-with-attachments.zip) doit être importé dans votre instance AEM pour que le formulaire fonctionne.



