---
title: Développer avec Output et Forms Services dans AEM Forms
description: Découvrez comment développer avec l’API Output et Forms Service dans AEM Forms.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
source-git-commit: 08ad6e3e6db6940f428568c749901b0b3c6ca171
workflow-type: ht
source-wordcount: '565'
ht-degree: 100%

---

# Développer avec Output et Forms Services dans AEM Forms{#developing-with-output-and-forms-services-in-aem-forms}

Découvrez comment développer avec l’API Output et Forms Service dans AEM Forms.

Dans cet article, nous examinerons ce qui suit.

* [Output Service](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) : en règle générale, ce service est utilisé pour fusionner des données XML avec un modèle xdp ou PDF, afin de générer un PDF aplati.
* [FormsService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) : il s’agit d’un service très polyvalent qui vous permet d’exporter/importer des données depuis et vers un fichier PDF.


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

[Téléchargez et installez le package à l’aide du gestionnaire de packages AEM.](assets/using-output-and-form-service-api.zip)




**Après avoir installé le package, vous devrez placer sur la liste autorisée les URL suivantes dans le filtre Adobe CSRF Granite.**

1. Suivez les étapes mentionnées ci-dessous pour placer sur la liste autorisée les chemins mentionnés ci-dessus.
1. [Se connecter à configMgr](http://localhost:4502/system/console/configMgr).
1. Recherchez un filtre CSRF Adobe Granite.
1. Ajoutez les 3 chemins suivants dans les sections exclues et enregistrez.
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. Recherchez le filtre référent Sling (« Sling Referrer filter »).
1. Cochez la case « Autoriser les champs vides ». (Ce paramètre doit être utilisé à des fins de test uniquement.)

## Tester les exemples

Il existe plusieurs façons de tester l’exemple de code. La plus rapide et la plus simple est d’utiliser l’application Postman. Postman vous permet d’effectuer des requêtes POST à votre serveur.

* Installez l’application Postman sur votre système.
* Lancez l’application et saisissez l’URL appropriée.
* Assurez-vous d’avoir sélectionné « POST » dans la liste déroulante.
* Veillez à spécifier « Autorisation » comme « Authentification de base ». Indiquez le nom d’utilisateur ou d’utilisatrice et le mot de passe du serveur AEM Server.
* Indiquez les paramètres de requête dans l’onglet de corps.
* Cliquez sur le bouton d’envoi.

Le package contient 4 exemples. Les paragraphes suivants expliquent à quel moment utiliser le service Output ou Forms Service, l’URL du service, les paramètres d’entrée attendus par chaque service.

## Utiliser OutputService pour fusionner des données avec le modèle xdp

* Utilisez le service Output pour fusionner des données avec un document XDP ou PDF pour générer un PDF aplati.
* **URL POST** : http://localhost:4502/content/AemFormsSamples/outputservice.html.
* **Paramètres de requête :**

   * **xdp_or_pdf_file** : fichier XDP ou PDF avec lequel vous souhaitez fusionner des données.
   * **xmlfile** : fichier de données XML fusionné avec xdp_or_pdf_file
   * **saveLocation** : emplacement où enregistrer le document rendu sur votre système de fichiers. Par exemple, C:\\documents\\sample.pdf.

### Utiliser lʼAPI FormsService

#### Importer les données

* Utilisez l’API importData FormsService pour importer des données dans un fichier PDF.
* **URL POST** : http://localhost:4502/content/AemFormsSamples/mergedata.html.

* **Paramètres de requête :**

   * **pdffile** : fichier PDF avec lequel vous souhaitez fusionner des données.
   * **xmlfile** : fichier de données XML fusionné avec le fichier PDF.
   * **saveLocation** : emplacement où enregistrer le document rendu sur votre système de fichiers. Par exemple, `c:\\outputsample.pdf`.

#### Exporter des données

* Utiliser l’API exportData FormsService pour exporter des données à partir d’un fichier PDF
* **URL POST** : http://localhost:4502/content/AemFormsSamples/exportdata.html
* **Paramètres de requête :**

   * **pdffile** : fichier pdf à partir duquel vous souhaitez exporter des données.
   * **saveLocation** : emplacement où enregistrer les données exportées sur votre système de fichiers. Par exemple, c:\\documents\\exported_data.xml.

#### Effectuer un rendu XDP

* Effectuer le rendu du modèle XDP en tant que PDF statique/dynamique
* Utiliser l’API renderPDFForm FormsService pour effectuer le rendu du modèle XDP en tant que PDF
* **URL POST** : http://localhost:4502/content/AemFormsSamples/renderxdp?
* Paramètre de requête :
   * xdpName : nom du fichier XDP à rendre en tant que PDF

[Vous pouvez importer cette collection Postman pour tester l’API.](assets/UsingDocumentServicesInAEMForms.postman_collection.json)
