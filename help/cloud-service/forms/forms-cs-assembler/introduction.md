---
title: Manipulation de PDF dans Forms CS à l’aide de l’opération invoke DDX
description: Assemblez les fichiers PDF à l’aide de l’opération invoke DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
source-git-commit: e925b9fa02dc8d4695b85377c5f7f43fbd45ebc8
workflow-type: ht
source-wordcount: '96'
ht-degree: 100%

---

# Présentation

Dans ce cours, nous utiliserons la manipulation de PDF et l’archivage de documents PDF à l’aide de Forms CS. L’utilisation de ces microservices à partir d’une application externe implique les étapes suivantes :

1. Générer les informations d’identification du service pour un compte technique AEM
1. Créer un jeton web JSON (JWT) à partir des informations d’identification du service et l’échanger contre un jeton d’accès
1. Configurer l’accès au compte technique dans AEM
1. Effectuez des appels HTTP à l’aide du jeton d’accès pour manipuler des fichiers PDF/générer et valider des fichiers PDF/A.
