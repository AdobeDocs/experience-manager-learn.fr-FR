---
title: Génération de plusieurs pdf à partir d’un fichier de données
description: OutputService fournit plusieurs méthodes pour créer des documents à l’aide d’une conception de formulaire et de données à fusionner avec la conception de formulaire. Découvrez comment générer plusieurs pdf à partir d’un fichier XML volumineux contenant plusieurs enregistrements individuels.
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 1%

---

# Génération d’un ensemble de documents de PDF à partir d’un fichier de données XML

OutputService fournit plusieurs méthodes pour créer des documents à l’aide d’une conception de formulaire et de données à fusionner avec la conception de formulaire. L’article suivant explique le cas pratique pour générer plusieurs pdf à partir d’un fichier XML volumineux contenant plusieurs enregistrements individuels.
Voici la capture d’écran d’un fichier xml contenant plusieurs enregistrements.

![multi-enregistrement-xml](assets/multi-record-xml.PNG)

Le fichier XML de données comporte 2 enregistrements. Chaque enregistrement est représenté par l’élément form1 . Ce fichier XML est transmis à OutputService. [méthode generatePDFOutputBatch](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) Nous obtenons la liste des documents pdf (un par enregistrement) La signature de la méthode generatePDFOutputBatch utilise les paramètres suivants :

* templates : carte contenant le modèle, identifié par une clé
* data - Mappage contenant des documents de données XML, identifiés par clé
* pdfOutputOptions - options de configuration de la génération pdf
* batchOptions - options de configuration du lot



## Détails du cas d’utilisation{#use-case-details}

Dans ce cas pratique, nous allons fournir une interface web simple pour télécharger le modèle et le fichier data(xml). Une fois le chargement des fichiers terminé et la demande du POST envoyée au servlet AEM. Ce servlet extrait les documents et appelle la méthode generatePDFOutputBatch du service Output. Les fichiers pdf générés sont compressés dans un fichier zip et mis à la disposition de l’utilisateur final pour les télécharger à partir du navigateur web.

## Code du servlet{#servlet-code}

Voici le fragment de code de la servlet. Le code extrait le modèle (xdp) et le fichier de données (xml) de la requête. Le fichier de modèle est enregistré dans le système de fichiers. Deux mappages sont créés : templateMap et dataFileMap qui contiennent respectivement le modèle et les fichiers xml(data). Un appel est alors effectué pour générer la méthode generateMultipleRecords du service DocumentServices.

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### Code de mise en oeuvre de l’interface{#Interface-Implementation-Code}

Le code suivant génère plusieurs fichiers pdf à l’aide de generatePDFOutputBatch de OutputService et renvoie un fichier zip contenant les fichiers pdf au servlet appelant.

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### Déployer sur votre serveur{#Deploy-on-your-server}

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous :

* [Télécharger et extraire le contenu du fichier zip vers votre système de fichiers](assets/mult-records-template-and-xml-file.zip).Ce fichier zip contient le modèle et le fichier de données XML.
* [Pointez votre navigateur vers la console web Felix.](http://localhost:4502/system/console/bundles)
* [Déployer le bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Déploiement du lot AEM FormsDocumentServices personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)Groupe .Custom qui génère les fichiers PDF à l’aide de l’API OutputService
* [Pointez votre navigateur sur le gestionnaire de modules.](http://localhost:4502/crx/packmgr/index.jsp)
* [Importez et installez le package](assets/generate-multiple-pdf-from-xml.zip). Ce package contient une page html qui permet de déposer le modèle et les fichiers de données.
* [Pointez votre navigateur sur MultiRecords.html.](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* Faites glisser et déposez ensemble le modèle et le fichier de données XML.
* Téléchargez le fichier zip créé. Ce fichier zip contient les fichiers pdf générés par le service output.

>[!NOTE]
>Il existe plusieurs façons de déclencher cette fonctionnalité. Dans cet exemple, nous avons utilisé une interface web pour déposer le modèle et le fichier de données pour démontrer la fonctionnalité.
