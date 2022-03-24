---
title: Manipulation de PDF dans Forms CS à l’aide de l’opération invoke DDX
description: Assemblez les fichiers du PDF à l’aide de invoke DDX.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# Présentation

Dans ce cours, nous utiliserons la manipulation et l’archivage PDF de documents PDF à l’aide de Forms CS. L&#39;utilisation de ces microservices à partir d&#39;une application externe implique les étapes suivantes :

1. Générer les informations d’identification du service pour un compte technique AEM
1. Créez un jeton Web JSON (JWT) à partir des informations d’identification du service et échangez-le pour un jeton d’accès.
1. Configurer l’accès au compte technique dans AEM
1. Effectuez des appels HTTP à l’aide du jeton d’accès pour manipuler des fichiers de PDF/générer et valider des fichiers de PDF/A.
