---
title: Fusionner les données avec le modèle XDP
description: Créer un document PDF en fusionnant les données avec le modèle
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '80'
ht-degree: 15%

---

# Fusionner les données avec le modèle XDP

L’étape suivante consiste à fusionner les données XML avec le modèle pour générer le PDF. Ce PDF est ensuite envoyé pour signature à l’aide d’Adobe Sign.

## Utilisation d’OutputService pour générer le PDF

La variable [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) de la méthode OutputService a été utilisée pour générer le PDF.
Le PDF généré a ensuite été envoyé pour signature à l’aide de l’API REST Adobe Sign.

