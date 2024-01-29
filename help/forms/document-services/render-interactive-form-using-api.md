---
title: Rendre des PDF interactifs à l’aide des services Forms dans AEM Forms
description: Utiliser l’API Forms Service dans AEM Forms pour effectuer le rendu d’un PDF interactif
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 94
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '341'
ht-degree: 100%

---

# Rendre des PDF interactifs à l’aide des services Forms dans AEM Forms

Utiliser l’API Forms Service dans AEM Forms pour effectuer le rendu d’un PDF interactif

Dans cet article, nous allons examiner le service suivant :

* FormsService : il s’agit d’un service très polyvalent qui vous permet d’exporter/importer des données à partir de et vers un fichier PDF et de générer également un PDF interactif en fusionnant des données XML dans un modèle XDP.

La documentation officielle du [javadoc pour l’API AEM Forms se trouve ici](https://helpx.adobe.com/fr/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html).

Le fragment de code suivant effectue le rendu du PDF interactif à l’aide de l’opération renderPDFForm de FormsService. Il s’agit du modèle schengen.xdp utilisé pour fusionner les données XML.

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

Ligne 1 : indique l’emplacement du dossier contenant le modèle XDP.

Lignes 2 à 4 : créez un élément PDFFormRenderOptions et définissez ses propriétés.

Ligne 7 : générez un PDF interactif à l’aide de l’opération de service renderPDFForm de FormsService.

Ligne 11 : renvoie le PDF interactif généré à l’application appelante.

**Tester l’exemple de package sur votre système**
1. [Téléchargez et installez DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Téléchargez et installez l’exemple de lot DocumentServices à l’aide de la console web Felix.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Téléchargez et installez le package à l’aide du gestionnaire de packages AEM.](assets/downloadinteractivepdffrommobileform.zip)

1. [Connectez-vous à configMgr](http://localhost:4502/system/console/configMgr).
1. Recherchez un filtre CSRF Adobe Granite.
1. Ajoutez le chemin suivant dans les sections exclues et enregistrez.
1. /bin/generateinteractivepdf
1. Recherchez _Service de mappage des profils utilisateurs de services Apache Sling_ et cliquez dessus pour ouvrir les propriétés.
   1. Cliquez sur l’icône *+* (plus) pour ajouter le mappage de service suivant.
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. Cliquez sur « Enregistrer ».
1. [Ouvrez le formulaire mobile](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content).
1. Renseignez quelques champs, puis cliquez sur le bouton ***Télécharger et remplir...*** bouton.
1. Le PDF interactif est à présent téléchargé sur votre système local.


L’exemple de package contient le profil personnalisé associé au formulaire mobile. Explorez le fichier [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Ce fichier JSP extrait les données du formulaire mobile et effectue une requête POST au servlet monté sur le chemin ***/bin/generateinteractivepdf***. Le servlet renvoie le PDF interactif à l’application appelante. Le code du fichier customtoolbar.jsp télécharge ensuite le fichier sur votre système local.
