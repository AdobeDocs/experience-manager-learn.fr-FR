---
title: Envoi de pièces jointes de formulaire adaptatif
description: Zip des pièces jointes de formulaire adaptatif et envoi à l’aide du composant d’envoi de courrier électronique
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
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 2%

---


# Présentation



Le cas d’utilisation courant consiste à compresser les pièces jointes du formulaire adaptatif et à les envoyer à l’aide du composant Envoyer un courrier électronique dans un processus AEM. Pour réaliser le cas d’utilisation, une étape de processus de workflow personnalisé a été écrite. Dans cette étape de processus personnalisée, un fichier zip contenant les pièces jointes de formulaire dans créé et stocké sous le dossier de charge utile dans un fichier nommé *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)


