---
title: Envoi de pièces jointes de formulaire adaptatif
description: Zip des pièces jointes de formulaire adaptatif et envoi à l’aide du composant d’envoi de courrier électronique
sub-product: formulaires
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: 22437e93cbf8f36d723dc573fa327562cb51b562
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# Présentation


Le cas d’utilisation courant consiste à compresser les pièces jointes du formulaire adaptatif et à les envoyer à l’aide du composant Envoyer un courrier électronique dans un processus AEM. Pour réaliser le cas d’utilisation, une étape de processus de workflow personnalisé a été écrite. Dans cette étape de processus personnalisée, un fichier zip est créé et les pièces jointes du formulaire sont ajoutées au fichier zip. Le fichier zip créé est stocké dans la variable de workflow nommée zippedfile.

