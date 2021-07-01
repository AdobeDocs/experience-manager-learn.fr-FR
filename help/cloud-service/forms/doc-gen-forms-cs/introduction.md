---
title: Microservices de génération de documents dans AEM Forms CS
description: Utilisez les microservices de génération de documents provenant d’une application externe.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Services de document
topic: Développement
kt: 7432
thumbnail: 332439.jpg
source-git-commit: 33cb3d18b744d9a8e54a87152079b42ed09212f2
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# Présentation

Dans ce cours, nous allons utiliser les microservices de génération de documents pour générer un pdf en fusionnant les données avec un modèle XDP. L&#39;utilisation de ces microservices à partir d&#39;une application externe implique les étapes suivantes :

1. Générer les informations d’identification du service pour un compte technique AEM
1. Créez un jeton Web JSON (JWT) à partir des informations d’identification du service et échangez-le pour un jeton d’accès.
1. Configurer l’accès au compte technique dans AEM
1. Lancer des appels HTTP à l’aide du jeton d’accès
