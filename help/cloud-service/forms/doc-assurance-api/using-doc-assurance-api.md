---
title: Utilisation de l’API DocAssurance
description: Exemple de code pour appeler l’API DocAssurance à l’aide des composants HTTP Apache dans Java
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-15508
source-git-commit: 97fbe450823c6122a25dc46c851296094894683e
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Utilisation de l’API DocAssurance

La variable [Service DocAssurance](https://developer.adobe.com/experience-manager-forms-cloud-service-developer-reference/api/docassurance/#tag/DocAssurance) permet d’effectuer diverses opérations de signature ou de chiffrement numériques avec des documents du PDF, tels que la signature, la certification, l’ajout de champs de signature, le chiffrement, le décryptage, etc.
Cet article vous fournit des fragments de code Java pour vous aider à commencer à utiliser l’API. Le fragment de code utilise le jeton d’accès. [Cet article explique les étapes nécessaires pour générer un jeton d’accès](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/doc-gen-formscs/introduction)


<span class="preview">Cette fonctionnalité est disponible sous le programme d’adoption précoce. Vous pouvez écrire à aem-forms-ea@adobe.com à partir de votre ID de courrier électronique officiel pour rejoindre le programme d’adoption précoce et demander l’accès à cette fonctionnalité.</span>


## Conditions préalables

* Expérience avec AEM Forms Cloud Service
* Expérience dans l’utilisation [Composants Apache HTTP](https://hc.apache.org/httpcomponents-client-4.5.x/)
* Accès à l’environnement du Cloud Service AEM Forms

## Document Inspect

Utilisez l’API d’inspection pour récupérer le type de sécurité sur un document de PDF donné. Le fragment de code suivant devrait vous aider à démarrer.

```java
...
File fileToInspect = new File("path_to_your_pdf_file)";
HttpPost httpPost = new HttpPost("<your_aem_forms_instance>/adobe/forms/document/assure/inspect");
httpPost.addHeader("Authorization", "Bearer " + accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToInspect);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
try
{
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)   
    {
        String json = EntityUtils.toString(response.getEntity(), StandardCharsets.UTF_8);
        log.info("The mode of encryption is  " + JsonParser.parseString(json).getAsJsonObject().get("mode").getAsString());
    }

} 
catch (Exception e)
{
   log.error(e.getMessage());
}
...
```


## Chiffrer le document

Utilisez l’API Encrypt pour chiffrer les documents pdf avec un mot de passe. L’exemple de fragment de code suivant chiffre un PDF donné.

```java
...
File fileToEncrypt = new File("path_to_your_pdf_file");
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer " + accessToken ");
MultipartEntityBuilder builder = MultipartEntityBuilder.create(); byte[] fileContent = FileUtils.readFileToByteArray(fileToEncrypt); builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
String config = "{\"mode\":\"ENCRYPT_WITH_PASSWORD\",\"params\":{\"openPassword\":\"adobe\",\"permPassword\":\"systems\",\"permissions\":[\"ALL_PERM\"]}}";
 builder.addTextBody("config", config, ContentType.APPLICATION_JSON);
try
 {
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)
    {
       InputStream generatedPDF = response.getEntity().getContent();
       byte[] bytes = IOUtils.toByteArray(generatedPDF);
       File encryptedFile = new File("c:\\aem_forms_cs_api\\encrypted.pdf");
       FileOutputStream outputStream = new FileOutputStream(encryptedFile);
       outputStream.write(bytes);
       outputStream.close();
    }

}
catch (Exception e)
 {
    log.error(e.getMessage());
 }

...
```

## Ajout d’un champ de signature au format pdf

Utilisez l’API Signfield pour ajouter une signature au PDF fourni. L’exemple de fragment de code suivant ajoute un champ de signature appelé SignHere à la page 4 du document.

```java
...
File pdfFile = new File(pdfFile1.getPath());
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer "+accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(pdfFile);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("field", "SignHere", ContentType.TEXT_PLAIN);
String rectangle = "{\"lowerLeftX\":1,\"lowerLeftY\":40,\"width\":100,\"height\":100}";
builder.addTextBody("rectangle", rectangle, ContentType.APPLICATION_JSON);
```


## Supprimer le chiffrement

Utilisez l’opération PUT sur l’API de chiffrement pour supprimer le chiffrement du PDF fourni. Le fragment de code Java suivant devrait vous aider à démarrer.

```java
...
File fileToDecrypt = new File("path_to_your_pdf_file");

HttpPut httpPut = new HttpPut(putURL);
httpPut.addHeader("Authorization", "Bearer " + accessToken);

MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToDecrypt);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("config", "systems", ContentType.TEXT_PLAIN);

try {
    HttpEntity entity = builder.build();
    httpPut.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPut);

if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File encryptionRemoved = new File("c:\\aem_forms_cs_api\\encryption_removed.pdf");
        FileOutputStream outputStream = new FileOutputStream(encryptionRemoved);
        outputStream.write(bytes);
        outputStream.close();
        httpclient.close();
    }
} catch (Exception e) {
    log.error(e.getMessage());
}
...
```

### Collection Postman

Une collection Postman de l’API peut être [téléchargé ici à des fins de test](assets/DocAssuranceAPI.postman_collection.json). Vous pouvez utiliser le type d’authentification Authentification de base ou Jeton porteur pour appeler l’API.
