---
title: Utilisation de fragments dans le service de sortie
description: Génération de documents pdf avec des fragments résidant dans le référentiel crx
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
source-git-commit: 747d1823ce1bc6670d1e80abcf6483ac921c0a01
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 2%

---

# Génération de documents pdf à l’aide de fragments{#developing-with-output-and-forms-services-in-aem-forms}


Dans cet article, nous allons utiliser le service output pour générer des fichiers pdf à l’aide de fragments xdp. Le xdp principal et les fragments résident dans le référentiel crx. Il est important d’imiter la structure de dossiers du système de fichiers dans AEM. Par exemple, si vous utilisez un fragment dans le dossier fragments de votre xdp, vous devez créer un dossier appelé **fragments** sous votre dossier de base dans AEM. Le dossier de base contient votre modèle xdp de base. Par exemple, si votre système de fichiers contient la structure suivante :
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* Le dossier xdpdocuments contient votre modèle de base et les fragments contenus dans **fragments** folder

Vous pouvez créer la structure requise à l’aide du [Interface utilisateur des formulaires et des documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

Voici la structure de dossiers de l’exemple xdp qui utilise 2 fragments.
![forms&amp;document](assets/fragment-folder-structure-ui.png)


* Service Output : en règle générale, ce service est utilisé pour fusionner des données XML avec un modèle xdp ou un pdf pour générer un pdf aplati. Pour plus d’informations, reportez-vous à la section [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) pour le service Output. Dans cet exemple, nous utilisons des fragments résidant dans le référentiel crx.


Le code suivant a été utilisé pour inclure des fragments dans le fichier du PDF.

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**Test de l’exemple de package sur votre système**

* [Téléchargez et importez les exemples de fichiers xdp dans AEM](assets/xdp-templates-fragments.zip)
* [Téléchargez et installez le module à l’aide du gestionnaire de modules AEM.](assets/using-fragments-assets.zip)
* [Les exemples xdp et fragments peuvent être téléchargés ici](assets/xdptemplates.zip)

**Après avoir installé le package, vous devrez placer sur la liste autorisée les URL suivantes dans Adobe Granite CSRF Filter.**

1. Suivez les étapes mentionnées ci-dessous pour placer sur la liste autorisée les chemins mentionnés ci-dessus.
1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Rechercher un filtre CSRF Adobe Granite
1. Ajoutez le chemin suivant dans les sections exclues et enregistrez
1. /content/AemFormsSamples/usingfragments

Il existe plusieurs façons de tester l’exemple de code. Le plus rapide et le plus simple est d’utiliser l’application Postman. Postman vous permet d’envoyer des requêtes de POST à votre serveur. Installez l’application Postman sur votre système.
Lancez l’application et saisissez l’URL suivante pour tester l’API d’exportation des données.

Assurez-vous que vous avez sélectionné &quot;POST&quot; dans la liste déroulante http://localhost:4502/content/AemFormsSamples/usingfragments.html Assurez-vous que vous spécifiez &quot;Autorisation&quot; comme &quot;Auth de base&quot;. Indiquez le nom d’utilisateur et le mot de passe du serveur AEM Accédez à l’onglet &quot;Corps&quot; et spécifiez les paramètres de requête comme illustré dans l’image ci-dessous.
![export](assets/using-fragment-postman.png)
Cliquez ensuite sur le bouton Envoyer

[Vous pouvez importer cette collection Postman pour tester l’API](assets/usingfragments.postman_collection.json)
