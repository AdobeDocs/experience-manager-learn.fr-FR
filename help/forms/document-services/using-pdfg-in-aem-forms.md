---
title: Utilisation de PDFG dans AEM Forms
seo-title: Utilisation de PDFG dans AEM Forms
description: Démonstration de la fonctionnalité de glisser-déposer pour créer un PDF à l’aide d’AEM Forms
seo-description: Démonstration de la fonctionnalité de glisser-déposer pour créer un PDF à l’aide d’AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 3%

---


# Utilisation de PDFG dans AEM Forms{#using-pdfg-in-aem-forms}

Démonstration de la fonctionnalité de glisser-déposer pour créer un PDF à l’aide d’AEM Forms

PDFG signifie PDF Generation. Cela signifie que vous pouvez convertir différents formats de fichier en PDF. Les documents Microsoft Office sont les plus courants. PDFG fait partie d’AEM Forms depuis la version 6.1.
[L’API javadoc pour PDFG est répertoriée ici](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Les ressources associées à cet article vous permettront de faire glisser des documents MS Office ou des fichiers JPG dans la zone de dépôt de la page HTML. Une fois le document déposé, il appelle le service PDFG, convertit le document au format PDF et l’enregistre sur le système de fichiers du serveur AEM.

Pour installer les ressources de démonstration, procédez comme suit :

1. Configurez PDFG comme indiqué dans ce document [ici](https://helpx.adobe.com/fr/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Veuillez suivre la documentation appropriée relative à votre version d’AEM Forms.
1. [Importez et installez les ressources liées à cet article à l’aide du gestionnaire de packages.](assets/createpdfgdemov2.zip)
1. [Accédez à post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin votre CRX
1. Modifiez l’emplacement d’enregistrement selon vos préférences (ligne 9).
1. Enregistrez vos modifications.
1. Ouvrez la [ page html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) pour faire glisser et déposer des fichiers en vue de la conversion.
1. Déposez un fichier Word ou jpg dans la zone de dépôt.
1. Le document d’entrée sera converti en PDF et enregistré au même emplacement que celui spécifié au point 4.

Le fragment de code suivant montre l’utilisation du service PDFG pour convertir des fichiers au format PDF.

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

