---
title: Utiliser des fragments dans le service de sortie avec dossier de contrôle
description: Générer des documents PDF avec des fragments résidant dans le référentiel crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-09-07T00:00:00Z
thumbnail: ecma-fragments.jpg
exl-id: 6b0bd2f1-b8ee-4f96-9813-8c11aedd3621
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '364'
ht-degree: 100%

---

# Générer un document PDF avec des fragments à l’aide d’un script ECMA{#developing-with-output-and-forms-services-in-aem-forms}


Dans cet article, nous allons utiliser le service Output pour générer des fichiers PDF à l’aide de fragments xdp. Le xdp principal et les fragments résident dans le référentiel crx. Il est important d’imiter la structure de dossiers du système de fichiers dans AEM. Par exemple, si vous utilisez un fragment dans le dossier de fragments de votre xdp, vous devez créer un dossier appelé **fragments** sous votre dossier de base dans AEM. Le dossier de base contient votre modèle xdp de base. Par exemple, si votre système de fichiers contient la structure suivante :
* c:\xdptemplates : contient votre modèle xdp de base.
* c:\xdptemplates\fragments : contient des fragments. Le modèle principal fait référence au fragment comme illustré ci-dessous.
  ![fragment-xdp](assets/survey-fragment.png).
* Le dossier xdpdocuments contient votre modèle de base et les fragments du dossier **fragments**.

Vous pouvez créer la structure requise à l’aide de l’[interface utilisateur des formulaires et des documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).

Voici la structure de dossiers de l’exemple d’xdp qui utilise 2 fragments.
![forms&amp;document](assets/fragment-folder-structure-ui.png).


* Service Output : en règle générale, ce service est utilisé pour fusionner des données XML avec un modèle xdp ou un PDF, afin de générer un PDF aplati. Pour plus d’informations, reportez-vous au [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) pour le service Output. Dans cet exemple, nous utilisons des fragments résidant dans le référentiel crx.


Le script ECMA suivant a été utilisé pour générer le PDF. Notez l’utilisation de ResourceResolver et de ResourceResolverHelper dans le code. ResourceResolver est nécessaire, car ce code s’exécute en dehors de tout contexte utilisateur.

```java
var inputMap = processorContext.getInputMap();
var itr = inputMap.entrySet().iterator();
var entry = inputMap.entrySet().iterator().next();
var xmlData = inputMap.get(entry.getKey());
log.info("Got XML Data File");

var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
log.info("Got service resolver");
var resourceResolver = aemDemoListings.getFormsServiceResolver();
//The ResourceResolverHelper execute's the following code within the context of the resourceResolver 
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
             //var statement = new Packages.com.adobe.aemfd.docmanager.Document("/content/dam/formsanddocuments/xdpdocuments/main.xdp",resourceResolver);
               var outputService = sling.getService(Packages.com.adobe.fd.output.api.OutputService);
            var pdfOutputOptions = new Packages.com.adobe.fd.output.api.PDFOutputOptions();
            pdfOutputOptions.setContentRoot("crx:///content/dam/formsanddocuments/xdpdocuments");
            pdfOutputOptions.setAcrobatVersion(Packages.com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
            var dataMergedDocument = outputService.generatePDFOutput("main.xdp",xmlData,pdfOutputOptions);
               //var dataMergedDocument = outputService.generatePDFOutput(statement,xmlData,pdfOutputOptions);
            processorContext.setResult("mergeddocument.pdf",dataMergedDocument);
            log.info("Generated the pdf document with fragments");
      }

 });
```

**Pour tester l’exemple de package sur votre système :**
* [Déployez le lot Developingwithserviceuser.](assets/DevelopingWithServiceUser.jar)
* Ajoutez l’entrée **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** dans la modification du service de mappeur d’utilisateur ou d’utilisatrice, comme illustré dans la capture d’écran ci-dessous.
  ![Modification du mappeur d’utilisateur ou d’utilisatrice.](assets/user-mapper-service-amendment.png)
* [Téléchargez et importez les exemples de fichiers XDP et de scripts ECMA](assets/watched-folder-fragments-ecma.zip).
Cela crée une structure de dossiers de contrôle dans votre dossier c:/fragmentsandoutputservice.

* [Extrayez le fichier de données d’exemple](assets/usingFragmentsSampleData.zip) et placez-le dans le dossier d’installation de votre dossier de contrôle (c:\fragmentsandoutputservice\install).

* Vérifiez que le fichier PDF généré est présent dans le dossier des résultats de la configuration du dossier de contrôle.
