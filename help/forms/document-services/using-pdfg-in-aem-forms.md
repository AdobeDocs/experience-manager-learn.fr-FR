---
title: Utilisation de PDFG en AEM Forms
seo-title: Utilisation de PDFG en AEM Forms
description: Démonstration de la fonctionnalité de glisser-déposer pour créer un fichier PDF à l’aide d’AEM Forms
seo-description: Démonstration de la fonctionnalité de glisser-déposer pour créer un fichier PDF à l’aide d’AEM Forms
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 3%

---


# Utilisation de PDFG en AEM Forms{#using-pdfg-in-aem-forms}

Démonstration de la fonctionnalité de glisser-déposer pour créer un fichier PDF à l’aide d’AEM Forms

PDFG signifie PDF Generation. Cela signifie que vous pouvez convertir divers formats de fichier en PDF. Les documents Microsoft Office sont les plus courants. PDFG fait partie de l&#39;AEM Forms depuis la version 6.1.
[L’API javadoc pour PDFG est répertoriée ici](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

Les ressources associées à cet article vous permettront de faire glisser des documents MS Office ou JPG dans la zone de dépôt de la page HTML. Une fois le document supprimé, il appelle le service PDFG et convertit le document en PDF et l’enregistre sur le système de fichiers de AEM Server.

Pour installer les ressources de démonstration, procédez comme suit :

1. Configurez PDFG comme indiqué dans ce document [ici](https://helpx.adobe.com/fr/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. Veuillez suivre la documentation appropriée relative à votre version AEM Forms.
1. [Importez et installez les actifs associés à cet article à l’aide du gestionnaire de packages.](assets/createpdfgdemov2.zip)
1. [Accédez à post.](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) jspin dans CRX
1. Modifier l&#39;emplacement de sauvegarde selon vos préférences (ligne 9)
1. Enregistrez vos modifications.
1. Ouvrez la [ page html](http://localhost:4502/content/DocumentServices/CreatePDFG.html) pour faire glisser et déposer les fichiers en vue de la conversion.
1. Déposez un fichier Word ou jpg dans la zone de dépôt.
1. Le document d’entrée est converti en PDF et enregistré au même emplacement que celui spécifié au point 4.

Le fragment de code suivant illustre l’utilisation du service PDFG pour convertir des fichiers au format PDF.

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

