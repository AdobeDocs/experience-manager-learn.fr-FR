---
title: Fusionner les données avec le modèle XDP
description: Créer un document PDF en fusionnant les données avec le modèle
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 6a865402-db3d-4e0e-81a0-a15dace6b7ab
duration: 15
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '80'
ht-degree: 100%

---

# Fusionner les données avec le modèle XDP

L’étape suivante consiste à fusionner les données XML avec le modèle pour générer le PDF. Ce PDF est ensuite envoyé pour signature à l’aide d’Adobe Sign.

## Utilisation d’OutputService pour générer le PDF

La méthode [generatePDF](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/output/api/OutputService.html#generatePDFOutput-com.adobe.aemfd.docmanager.Document-com.adobe.aemfd.docmanager.Document-com.adobe.fd.output.api.PDFOutputOptions-) d’OutputService a été utilisée pour générer le PDF.
Le PDF généré a ensuite été envoyé pour signature à l’aide de l’API REST Adobe Sign.
