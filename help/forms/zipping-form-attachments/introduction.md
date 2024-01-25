---
title: Envoyer des pièces jointes de formulaire adaptatif
description: Envoyer des pièces jointes de formulaire adaptatif à l’aide du composant Envoyer un e-mail
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
duration: 30
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 100%

---

# Présentation



Le cas d’utilisation courant consiste à envoyer les pièces jointes de formulaire adaptatif à l’aide du composant Envoyer un e-mail dans un workflow AEM.
En règle générale, les clientes et clients mettent les pièces jointes de formulaire dans un fichier zip ou envoient les pièces jointes sous la forme de fichiers individuels à l’aide du composant Envoyer un e-mail.

## Envoyer des pièces jointes de formulaire via un fichier zip

Pour réaliser ce cas d’utilisation, une étape de processus de workflow personnalisée a été ajoutée. Au cours de cette étape de processus personnalisée, un fichier zip contenant les pièces jointes de formulaire est créé et stocké sous le dossier de payload dans un fichier nommé *zipped_attachments.zip*.

![send-form-attachments](assets/send-form-attachments.JPG)

## Envoyer les pièces jointes de formulaire individuellement

Pour réaliser ce cas d’utilisation, une étape de processus de workflow personnalisée a été ajoutée. Dans cette étape de processus personnalisée, nous renseignons les variables de workflow de type ArrayList de documents et ArrayList de chaînes.

![send-list-of-documents](assets/send-list-of-documents.JPG)

## Étapes suivantes

[Pièces jointes du formulaire Zip](./custom-process-step.md)
