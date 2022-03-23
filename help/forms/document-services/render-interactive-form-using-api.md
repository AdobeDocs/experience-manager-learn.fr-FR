---
title: Rendu du PDF interactif à l’aide des services Forms dans AEM Forms
description: Utilisation de l’API Forms Service dans AEM Forms pour effectuer le rendu d’un PDF interactif
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 1%

---

# Rendu du PDF interactif à l’aide des services Forms dans AEM Forms

Utilisation de l’API Forms Service dans AEM Forms pour effectuer le rendu d’un PDF interactif

Dans cet article, nous allons examiner le service suivant :

* FormsService - Il s’agit d’un service très polyvalent qui vous permet d’exporter/importer des données depuis et vers un fichier PDF et de générer également un pdf interactif en fusionnant des données XML dans un modèle xdp.

Le javadoc officiel pour l’API AEM Forms est répertorié. [here](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Le fragment de code suivant effectue le rendu du pdf interactif à l’aide de l’opération renderPDFForm de FormsService. Le schéma.xdp est un modèle utilisé pour fusionner les données XML.

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

Ligne 1 : Emplacement du dossier contenant le modèle xdp

Line2-4 : Créez PDFFormRenderOptions et définissez ses propriétés.

Ligne 7 : Générer un PDF interactif à l’aide de l’opération de service renderPDFForm de FormsService

Ligne 11 : Renvoie le pdf interactif généré à l’application appelante

**Test de l’exemple de package sur votre système**
1. [Téléchargez et installez l’exemple de bundle DocumentServices à l’aide de la console web Felix.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Téléchargez et installez le module à l’aide du gestionnaire de modules AEM.](assets/downloadinteractivepdffrommobileform.zip)



1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Rechercher un filtre CSRF Adobe Granite
1. Ajoutez le chemin suivant dans les sections exclues et enregistrez
1. /bin/generateinteractivepdf
1. [Ouvrir le formulaire mobile](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Renseignez quelques champs, puis cliquez sur le bouton ***Télécharger et remplir ....*** button
1. Le PDF interactif doit être téléchargé sur votre système local.


L’exemple de package contient le profil personnalisé associé au formulaire Mobile. Veuillez explorer la [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) fichier . Ce fichier jsp extrait les données du formulaire mobile et envoie une demande de POST au servlet monté sur ***/bin/generateinteractivepdf*** chemin d’accès. Le servlet renvoie le pdf interactif à l’application appelante. Le code du fichier customtoolbar.jsp télécharge ensuite le fichier sur votre système local.
