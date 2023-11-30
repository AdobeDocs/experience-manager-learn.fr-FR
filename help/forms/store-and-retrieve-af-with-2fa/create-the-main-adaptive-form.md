---
title: Créer le formulaire adaptatif principal
description: Créer des formulaires adaptatifs pour capturer les informations sur la personne demandeuse et le formulaire adaptatif pour récupérer le formulaire adaptatif enregistré
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 100%

---

# Créer le formulaire adaptatif principal

Le formulaire **StoreAFWithAttachments** est le formulaire adaptatif principal. Ce formulaire adaptatif est le point d’entrée du cas d’utilisation. Dans ce formulaire, les détails de l’utilisateur ou de l’utilisatrice, y compris le numéro de mobile, sont capturés. Ce formulaire permet également d’ajouter des pièces jointes. Lorsque l’utilisateur ou l’utilisatrice clique sur le bouton Enregistrer et quitter, le code côté serveur est exécuté pour stocker les données de formulaire dans la base de données. Un identifiant d’application unique est généré et présenté à l’utilisateur ou l’utilisatrice afin d’être sécurisé. Cet identifiant d’application est utilisé pour récupérer le numéro de mobile associé à l’application.

![Formulaire d’application principal.](assets/6552.JPG)

Ce formulaire est associé aux bibliothèques clientes **bootboxjs540,storeAFWithAttachments** créées plus tôt dans le cours et à un workflow d’AEM qui est déclenché lors de l’envoi du formulaire.


* Les exemples de formulaires reposent sur le [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* Le [formulaire StoreAfWithAttachments](assets/store-af-with-attachments-form.zip) renseigné peut être téléchargé et importé dans votre instance AEM.

* Le [workflow AEM associé à ce formulaire](assets/workflow-model-store-af-with-attachments.zip) doit être importé dans votre instance AEM pour que le formulaire fonctionne.


## Étapes suivantes

[Créer le formulaire en récupérant le formulaire enregistré](./retrieve-saved-form.md)