---
title: Envoi de pièces jointes de formulaire adaptatif
description: Envoi de pièces jointes de formulaire adaptatif à l’aide du composant Envoyer un courrier électronique
feature: formulaires adaptatifs
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: Développement
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 1%

---


# Présentation



Le cas d’utilisation courant consiste à envoyer les pièces jointes de formulaire adaptatif à l’aide du composant d’envoi de courrier électronique dans un processus AEM.
En règle générale, les clients envoient les pièces jointes du formulaire ou les pièces jointes sous la forme de fichiers individuels à l’aide du composant d’envoi d’email.

## Envoi des pièces jointes du formulaire dans un fichier zip

Pour réaliser le cas d’utilisation, une étape de processus de workflow personnalisé a été écrite. Dans cette étape de processus personnalisée, un fichier zip contenant les pièces jointes de formulaire dans créé et stocké sous le dossier de charge utile dans un fichier nommé *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## Envoyer les pièces jointes du formulaire individuellement

Pour réaliser ce cas d’utilisation, une étape de processus de workflow personnalisé a été écrite. Dans cette étape de processus personnalisée, nous renseignons les variables de workflow de type ArrayList de documents et ArrayList de chaînes.

![send-list-of-documents](assets/send-list-of-documents.JPG)



