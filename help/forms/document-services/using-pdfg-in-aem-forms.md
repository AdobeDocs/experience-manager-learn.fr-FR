---
title: Utiliser PDFG dans AEM Forms
description: Démontrer la fonctionnalité de glisser-déposer pour créer un PDF à l’aide d’AEM Forms
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
duration: 76
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 100%

---

# Utiliser PDFG dans AEM Forms{#using-pdfg-in-aem-forms}

Démontrer la fonctionnalité de glisser-déposer pour créer un PDF à l’aide d’AEM Forms

PDFG signifie Génération de PDF. Cela signifie que vous pouvez convertir divers formats de fichier en PDF. Les documents les plus courants sont les documents Microsoft Office. PDFG appartient à AEM Forms depuis la version 6.1.
[L’API JavaDoc pour PDFG est répertoriée ici](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html).

Les ressources associées à cet article vous permettent de glisser et de déposer des documents MS Office ou un fichier JPG dans la zone de dépôt de la page HTML. Une fois le document déposé, il appelle le service PDFG, convertit le document en PDF et l’enregistre sur le système de fichiers du serveur AEM.

Pour installer les ressources de démonstration, procédez comme suit :

1. Configurez PDFG comme indiqué dans ce document [ici](https://helpx.adobe.com/fr/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Suivez la documentation appropriée relative à votre version d’AEM Forms.
1. [Importez et installez les ressources liées à cet article à l’aide du gestionnaire de packages.](assets/createpdfgdemov2.zip)
1. [Accédez à post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) dans votre CRX.
1. Modifiez l’emplacement d’enregistrement selon vos préférences (ligne 9).
1. Enregistrez vos modifications.
1. Ouvrez la [page HTML](http://localhost:4502/content/DocumentServices/CreatePDFG.html) pour glisser et déposer des fichiers à convertir.
1. Déposez un fichier Word ou JPG dans la zone de dépôt.
1. Le document d’entrée est converti en PDF et enregistré au même emplacement que celui spécifié au point 4.

L’extrait de code suivant montre l’utilisation du service PDFG pour convertir des fichiers en PDF.

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
