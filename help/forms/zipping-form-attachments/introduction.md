---
title: Envoi de pièces jointes de formulaire adaptatif
description: Envoi de pièces jointes de formulaire adaptatif à l’aide du composant Envoyer un courrier électronique
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Présentation 



Le cas d’utilisation courant consiste à envoyer les pièces jointes de formulaire adaptatif à l’aide du composant d’envoi de courrier électronique dans un processus AEM.
En règle générale, les clients envoient les pièces jointes du formulaire ou les pièces jointes sous la forme de fichiers individuels à l’aide du composant d’envoi d’email.

## Envoi des pièces jointes du formulaire dans un fichier zip

Pour réaliser le cas d’utilisation, une étape de processus de workflow personnalisé a été écrite. Au cours de cette étape de processus personnalisé, un fichier zip contenant les pièces jointes du formulaire dans créé et stocké sous le dossier de charge utile dans un fichier nommé *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## Envoyer les pièces jointes du formulaire individuellement

Pour réaliser ce cas d’utilisation, une étape de processus de workflow personnalisé a été écrite. Dans cette étape de processus personnalisée, nous renseignons les variables de workflow de type ArrayList de documents et ArrayList de chaînes.

![send-list-of-documents](assets/send-list-of-documents.JPG)
