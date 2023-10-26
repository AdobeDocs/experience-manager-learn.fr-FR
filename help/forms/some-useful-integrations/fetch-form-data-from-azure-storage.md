---
title: Stocker l’envoi du formulaire dans Azure Storage
description: Stockage des données de formulaire dans Azure Storage à l’aide de l’API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 2%

---

# Récupérer des données à partir du stockage Azure

Cet article vous explique comment remplir un formulaire adaptatif avec les données stockées dans le stockage Azure.
Nous partons du principe que vous avez stocké les données de formulaire adaptatif dans le stockage Azure et que vous souhaitez maintenant préremplir votre formulaire adaptatif avec ces données.

## Créer une requête de GET

L’étape suivante consiste à écrire le code pour récupérer les données du stockage Azure à l’aide de l’blobID. Le code suivant a été écrit pour récupérer les données. L’URL a été créée à l’aide des valeurs sasToken et storageURI de la configuration OSGi et de l’blobID transmis à la fonction getBlobData .

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.error("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.error("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Lorsqu’un formulaire adaptatif est rendu avec un `guid` dans l’URL, le composant de page personnalisé associé au modèle récupère et renseigne le formulaire adaptatif avec les données du stockage Azure.
Le composant de page associé au modèle comporte le code JSP suivant.

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## Tester la solution

* [Déploiement du lot OSGi personnalisé](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importez le modèle de formulaire adaptatif personnalisé et le composant de page associé au modèle.](./assets/store-and-fetch-from-azure.zip)

* [Import de l’exemple de formulaire adaptatif](./assets/bank-account-sample-form.zip)

* Spécifiez les valeurs appropriées dans la configuration du portail Azure à l’aide de la console de configuration OSGi.
* [Aperçu et envoi du formulaire BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Vérifiez que les données sont stockées dans le conteneur de stockage Azure de votre choix. Copiez l’ID de Blob.
* [Aperçu du formulaire BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) et spécifiez l’ID Blob comme paramètre guid dans l’URL pour que le formulaire soit prérempli avec les données du stockage Azure.

