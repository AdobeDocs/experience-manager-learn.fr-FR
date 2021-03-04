---
title: Développement avec les services Output et Forms à AEM Forms
seo-title: Développement avec les services Output et Forms à AEM Forms
description: Utilisation de l’API Output et du service Forms dans AEM Forms
seo-description: Utilisation de l’API Output et du service Forms dans AEM Forms
feature: Service Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 2%

---


# Rendu de fichiers PDF interactifs à l’aide des services Forms dans AEM Forms

Utilisation de l’API Forms Service dans AEM Forms pour générer un PDF interactif

Dans cet article, nous examinerons le service suivant :

* FormsService - Il s’agit d’un service très polyvalent qui vous permet d’exporter/d’importer des données de et dans un fichier PDF et de générer des fichiers PDF interactifs en fusionnant des données xml dans un modèle xdp.

L’API javadoc officielle pour AEM Forms est répertoriée [ici](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Le fragment de code suivant affiche un fichier pdf interactif à l’aide de l’opération renderPDFForm de FormsService. Le fichier schengen.xdp est un modèle utilisé pour fusionner les données xml.

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

Ligne 2-4 : Création de PDFFormRenderOptions et définition de ses propriétés

Ligne 7 : Générer un fichier PDF interactif à l’aide de l’opération de service renderPDFForm de FormsService

Ligne 11 : Renvoie le pdf interactif généré à l’application appelante.

**Pour tester l’exemple de package sur votre système**
1. [Téléchargement et installation de l’exemple de lot Document Services à l’aide de la console Web Felix](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Téléchargez et installez le package à l’aide du gestionnaire de packages AEM.](assets/downloadinteractivepdffrommobileform.zip)



1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Rechercher un filtre CSRF Granite Adobe
1. Ajoutez le chemin suivant dans les sections exclues et enregistrez
1. /bin/generateinteractivepdf
1. [Ouverture du formulaire pour périphériques mobiles](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. Renseignez quelques champs, puis cliquez sur le ***Télécharger et remplir ....*** button
1. Le pdf interactif doit être téléchargé sur votre système local.


L’exemple de module contient le profil personnalisé associé au formulaire pour périphériques mobiles. Explorez le fichier [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp). Ce fichier jsp extrait les données du formulaire mobile et envoie une requête de POST au servlet monté sur le chemin ***/bin/generateinteractivepdf***. La servlet renvoie le pdf interactif à l’application appelante. Le code du fichier customtoolbar.jsp télécharge ensuite le fichier sur votre système local.


