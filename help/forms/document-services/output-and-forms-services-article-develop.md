---
title: Développer avec les services Output et Forms dans AEM Forms
description: Utiliser l’API des services Output et Forms dans AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: ht
source-wordcount: '596'
ht-degree: 100%

---

# Développer avec les services Output et Forms dans AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Utiliser l’API des services Output et Forms dans AEM Forms

Dans cet article, nous examinerons ce qui suit.

* Service Output : en règle générale, ce service est utilisé pour fusionner des données XML avec un modèle xdp ou un PDF, afin de générer un PDF aplati. Pour plus d’informations, reportez-vous à ce [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) pour le service Output.
* FormsService : il s’agit d’un service très polyvalent qui vous permet d’exporter/importer des données depuis et vers un fichier PDF. Pour plus d’informations, reportez-vous à ce [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) pour le service Forms.


L’extrait de code suivant exporte les données depuis un fichier PDF.

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La ligne 1 extrait le fichier PDF à partir de la requête.

La ligne 2 extrait l’emplacement d’enregistrement à partir de la requête.

La ligne 5 contient le service Forms.

La ligne 6 exporte les données XML xmlData du fichier PDF.

**Pour tester l’exemple de package sur votre système :**

[Téléchargez et installez le package à l’aide du gestionnaire de packages AEM.](assets/outputandformsservice.zip)




**Après avoir installé le package, vous devrez placer sur la liste autorisée les URL suivantes dans le filtre Adobe CSRF Granite.**

1. Suivez les étapes mentionnées ci-dessous pour placer sur la liste autorisée les chemins mentionnés ci-dessus.
1. [Se connecter à configMgr](http://localhost:4502/system/console/configMgr).
1. Recherchez un filtre CSRF Adobe Granite.
1. Ajoutez les 3 chemins suivants dans les sections exclues et enregistrez.
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Recherchez le filtre référent Sling (« Sling Referrer filter »).
1. Cochez la case « Autoriser les champs vides ». (Ce paramètre doit être utilisé à des fins de test uniquement.)
Il existe plusieurs façons de tester l’exemple de code. La plus rapide et la plus simple est d’utiliser l’application Postman. Postman vous permet d’envoyer des requêtes POST à votre serveur. Installez l’application Postman sur votre système.
Lancez l’application et saisissez l’URL suivante pour tester l’API d’export des données.

Assurez-vous de sélectionner « POST » dans la liste déroulante.
http://localhost:4502/content/AemFormsSamples/exportdata.html
Spécifiez « Autorisation » comme authentification de base. Indiquez le nom d’utilisateur ou d’utilisatrice et le mot de passe du serveur AEM.
Accédez à l’onglet « Corps » et spécifiez les paramètres de requête comme illustré dans l’image ci-dessous.
![export](assets/postexport.png)
Cliquez ensuite sur le bouton Envoyer.

Le package contient 3 exemples. Les paragraphes suivants expliquent à quel moment utiliser le service de sortie ou le service Forms, l’URL du service, les paramètres d’entrée attendus par chaque service.

## Fusionner les données et aplatir la sortie

* Utilisez le service Output pour fusionner des données avec un document XDP ou PDF pour générer un PDF aplati.
* **URL POST** : http://localhost:4502/content/AemFormsSamples/outputservice.html.
* **Paramètres de requête :**

   * **xdp_or_pdf_file** : fichier XDP ou PDF avec lequel vous souhaitez fusionner des données.
   * **xmlfile** : fichier de données XML fusionné avec xdp_or_pdf_file
   * **saveLocation** : emplacement où enregistrer le document rendu sur votre système de fichiers. Par exemple, C:\\documents\\sample.pdf.

### Importer des données dans un fichier PDF

* Utilisez le service Forms pour importer des données dans un fichier PDF.
* **URL POST** : http://localhost:4502/content/AemFormsSamples/mergedata.html.
* **Paramètres de requête :**

   * **pdffile** : fichier PDF avec lequel vous souhaitez fusionner des données.
   * **xmlfile** : fichier de données XML fusionné avec le fichier PDF.
   * **saveLocation** : emplacement où enregistrer le document rendu sur votre système de fichiers. Par exemple, `c:\\outputsample.pdf`.

**Exporter des données du formulaire PDF**
* Utiliser FormsService pour exporter des données à partir d’un fichier PDF
* **URL POST** : http://localhost:4502/content/AemFormsSamples/exportdata.html.
* **Paramètres de requête :**

   * **pdffile** : fichier pdf à partir duquel vous souhaitez exporter des données.
   * **saveLocation** : emplacement où enregistrer les données exportées sur votre système de fichiers. Par exemple, c:\\documents\\exported_data.xml.

[Vous pouvez importer cette collection Postman pour tester l’API.](assets/document-services-postman-collection.json)
