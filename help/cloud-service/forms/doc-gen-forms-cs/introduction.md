---
title: Microservices de génération de documents dans AEM Forms CS
description: Utilisez les microservices de génération de documents provenant d’une application externe.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 7432
thumbnail: 334859.jpg
exl-id: 08dd141d-9d25-4dd5-a810-70e3835deae4
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---

# Présentation

Dans ce cours, nous allons utiliser les microservices de génération de documents pour générer un pdf en fusionnant les données avec un modèle XDP. L&#39;utilisation de ces microservices à partir d&#39;une application externe implique les étapes suivantes :

1. Générer les informations d’identification du service pour un compte technique AEM
1. Créez un jeton Web JSON (JWT) à partir des informations d’identification du service et échangez-le pour un jeton d’accès.
1. Configurer l’accès au compte technique dans AEM
1. Lancer des appels HTTP à l’aide du jeton d’accès

>[!VIDEO](https://video.tv.adobe.com/v/334859?quality=12&learn=on)
