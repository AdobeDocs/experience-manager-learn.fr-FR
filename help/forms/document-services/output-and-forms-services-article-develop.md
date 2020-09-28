---
title: Développement avec les services Output et Forms à AEM Forms
seo-title: Développement avec les services Output et Forms à AEM Forms
description: Utilisation de l’API Output et du service Forms dans AEM Forms
seo-description: Utilisation de l’API Output et du service Forms dans AEM Forms
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 1%

---


# Développement avec les services Output et Forms à AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Utilisation de l’API Output et du service Forms dans AEM Forms

Dans cet article, nous examinerons ce qui suit :

* Output Service - Ce service est généralement utilisé pour fusionner des données xml avec un modèle xdp ou un fichier pdf afin de générer un fichier pdf aplati.
* FormsService - Il s’agit d’un service très polyvalent qui vous permet d’exporter/d’importer des données de et dans un fichier PDF.

L’API javadoc officielle pour AEM Forms est répertoriée [ici.](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

Le fragment de code suivant exporte les données du fichier PDF.

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La ligne 1 extrait le fichier de la requête

Line2 extrait le saveLocation de la requête.

La ligne 5 récupère FormsService

La ligne 6 exporte les xmlData du fichier PDF.

**Pour tester l’exemple de package sur votre système**

[Téléchargez et installez le package à l’aide du gestionnaire de packages AEM.](assets/outputandformsservice.zip)




**Après avoir installé le pack, vous devrez placer sur la liste autorisée les URL suivantes dans le filtre CSRF Granite Adobe.**

1. Suivez les étapes mentionnées ci-dessous pour placer sur la liste autorisée les chemins mentionnés ci-dessus.
1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Rechercher un filtre CSRF Granite Adobe
1. ajoutez les 3 chemins suivants dans les sections exclues et enregistrez
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Rechercher &quot;Sling Parrain filter&quot;
1. Cochez la case &quot;Autoriser les champs vides&quot;. (Ce paramètre doit être utilisé à des fins de test uniquement) Il existe plusieurs méthodes pour tester l’exemple de code. Le plus rapide et le plus simple est d&#39;utiliser l&#39;application Postman. Postman vous permet d&#39;envoyer des demandes POST à votre serveur. Installez l’application Postman sur votre système.
Lancez l’application et saisissez l’URL suivante pour tester l’API d’exportation des données.

Assurez-vous d&#39;avoir sélectionné &quot;POST&quot; dans la liste déroulante http://localhost:4502/content/AemFormsSamples/exportdata.htmlAssurez-vous de spécifier &quot;Autorisation&quot; comme &quot;Auth de base&quot;. Indiquez le nom d&#39;utilisateur et le mot de passe AEM ServerAccédez à l&#39;onglet &quot;Body&quot; (Corps) et spécifiez les paramètres de requête, comme illustré dans l&#39;image ci-dessous![export](assets/postexport.png), puis cliquez sur le bouton Envoyer.

Le paquet contient 3 exemples. Les paragraphes suivants expliquent à quel moment utiliser le service de sortie ou Forms Service, l’URL du service, les paramètres d’entrée attendus par chaque service.

**Fusionner les données et aplatir la sortie :**

* Utiliser Output Service pour fusionner des données avec xdp ou pdf document pour générer un pdf aplati
* **URL** du POST : http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Paramètres de la demande -**

   * xdp_or_pdf_file : Fichier xdp ou pdf avec lequel vous souhaitez fusionner des données
   * xmlfile: Le fichier de données xml qui sera fusionné avec xdp_or_pdf_file
   * saveLocation : Emplacement d’enregistrement du document rendu sur votre système de fichiers

**Importer des données dans un fichier PDF :**
* Utilisation de FormsService pour importer des données dans un fichier PDF
* **URL** du POST - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Paramètres de la demande:**

   * pdffile : Le fichier pdf avec lequel vous souhaitez fusionner des données
   * xmlfile: Fichier de données xml qui sera fusionné avec le fichier pdf
   * saveLocation : Emplacement d’enregistrement du document rendu sur votre système de fichiers. Par exemple c:\\\outputsample.pdf.

**Exporter des données à partir d’un fichier PDF**
* Utilisation de FormsService pour exporter des données à partir d’un fichier PDF
* **** URL du POST - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Paramètres de la demande:**

   * pdffile : Le fichier pdf à partir duquel vous souhaitez exporter des données
   * saveLocation : Emplacement d&#39;enregistrement des données exportées sur votre système de fichiers

[Vous pouvez importer cette collection de facteur pour tester l&#39;API](assets/document-services-postman-collection.json)

