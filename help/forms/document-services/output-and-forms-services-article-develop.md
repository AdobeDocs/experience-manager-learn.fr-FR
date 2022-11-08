---
title: Développement avec les services Output et Forms dans AEM Forms
description: Utilisation de l’API Output et Forms Service dans AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 2%

---

# Développement avec les services Output et Forms dans AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Utilisation de l’API Output et Forms Service dans AEM Forms

Dans cet article, nous examinerons ce qui suit :

* Service Output : en règle générale, ce service est utilisé pour fusionner des données XML avec un modèle xdp ou un pdf pour générer un pdf aplati. Pour plus d’informations, reportez-vous à cette section [javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) pour le service Output.
* FormsService : il s’agit d’un service très polyvalent qui vous permet d’exporter/importer des données depuis et vers un fichier PDF. Pour plus d’informations, reportez-vous à cette section [javadoc](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) pour le service Forms.


Le fragment de code suivant exporte les données du fichier de PDF.

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

La ligne 1 extrait le fichier de la requête.

Line2 extrait le saveLocation de la requête.

La ligne 5 contient FormsService

La ligne 6 exporte les xmlData du fichier du PDF

**Test de l’exemple de package sur votre système**

[Téléchargez et installez le module à l’aide du gestionnaire de modules AEM.](assets/outputandformsservice.zip)




**Après avoir installé le package, vous devrez placer sur la liste autorisée les URL suivantes dans Adobe Granite CSRF Filter.**

1. Suivez les étapes mentionnées ci-dessous pour placer sur la liste autorisée les chemins mentionnés ci-dessus.
1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Rechercher un filtre CSRF Adobe Granite
1. Ajoutez les 3 chemins suivants dans les sections exclues et enregistrez
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. Recherchez &quot;Sling Referrer filter&quot;.
1. Cochez la case &quot;Autoriser vide&quot;. (Ce paramètre doit être utilisé à des fins de test uniquement) Il existe plusieurs façons de tester l’exemple de code. Le plus rapide et le plus simple est d’utiliser l’application Postman. Postman vous permet d’envoyer des requêtes de POST à votre serveur. Installez l’application Postman sur votre système.
Lancez l’application et saisissez l’URL suivante pour tester l’API d’exportation des données.

Assurez-vous que vous avez sélectionné &quot;POST&quot; dans la liste déroulante http://localhost:4502/content/AemFormsSamples/exportdata.html Assurez-vous que vous spécifiez &quot;Autorisation&quot; comme &quot;Auth de base&quot;. Indiquez le nom d’utilisateur et le mot de passe du serveur AEM Accédez à l’onglet &quot;Corps&quot; et spécifiez les paramètres de requête comme illustré dans l’image ci-dessous.
![export](assets/postexport.png)
Cliquez ensuite sur le bouton Envoyer

Le package contient 3 exemples. Les paragraphes suivants expliquent à quel moment utiliser le service de sortie ou Forms Service, l’URL du service, les paramètres d’entrée attendus par chaque service.

## Fusionner les données et aplatir la sortie

* Utilisation de Output Service pour fusionner des données avec xdp ou pdf document pour générer un pdf aplati.
* **URL du POST**: http://localhost:4502/content/AemFormsSamples/outputservice.html
* **Paramètres de la demande -**

   * **xdp_or_pdf_file** : Le fichier xdp ou pdf avec lequel vous souhaitez fusionner des données.
   * **xmlfile**: Le fichier de données XML fusionné avec xdp_or_pdf_file
   * **saveLocation**: L’emplacement où enregistrer le document rendu sur votre système de fichiers. Par exemple c:\\documents\\sample.pdf

### Importer des données dans un fichier PDF

* Utilisation de FormsService pour importer des données dans un fichier PDF
* **URL du POST** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **Paramètres de la demande:**

   * **pdfile** : Le fichier pdf avec lequel vous souhaitez fusionner des données
   * **xmlfile**: Le fichier de données XML fusionné avec le fichier pdf
   * **saveLocation**: L’emplacement où enregistrer le document rendu sur votre système de fichiers. Par exemple, `c:\\outputsample.pdf`.

**Exporter des données à partir d’un fichier PDF**
* Utilisation de FormsService pour exporter des données à partir d’un fichier de PDF
* **URL du POST** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Paramètres de la demande:**

   * **pdfile** : Le fichier pdf à partir duquel vous souhaitez exporter des données
   * **saveLocation**: L’emplacement où enregistrer les données exportées sur votre système de fichiers. Par exemple c:\\documents\\exported_data.xml

[Vous pouvez importer cette collection Postman pour tester l’API](assets/document-services-postman-collection.json)
