---
title: Utilisation de PDFG dans AEM Forms
description: Démonstration de la fonctionnalité de glisser-déposer pour créer un PDF à l’aide d’AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 4%

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

