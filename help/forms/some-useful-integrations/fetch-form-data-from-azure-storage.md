---
title: Stocker l’envoi de formulaire dans le stockage Azure
description: Stocker les données de formulaire dans le stockage Azure à l’aide de l’API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
jira: KT-14238
duration: 108
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 75%

---

# Récupérer des données à partir du stockage Azure

Dans cet article, découvrez comment remplir un formulaire adaptatif avec les données stockées dans le stockage Azure.
Avant de poursuivre votre lecture, assurez-vous d’avoir stocké les données de formulaire adaptatif dans le stockage Azure. Ces données permettront de préremplir les formulaires adaptatifs.

## Créer une requête GET

L’étape suivante consiste à écrire le code permettant de récupérer les données du stockage Azure à l’aide de l’ID d’objet blob. Le code suivant permet de récupérer les données. L’URL a été créée à l’aide des valeurs sasToken et storageURI de la configuration OSGi et de l’ID d’objet blob transmis à la fonction getBlobData.

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

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

Lorsqu’un formulaire adaptatif est rendu avec un paramètre guid dans l’URL, le composant de page personnalisé associé au modèle récupère et renseigne le formulaire adaptatif avec les données du stockage Azure.
Voici le code dans le fichier jsp du composant de page associé au modèle.

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

* [Déployez le lot OSGi personnalisé.](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [Importez le modèle de formulaire adaptatif personnalisé et le composant de page associé à celui-ci.](./assets/store-and-fetch-from-azure.zip)

* [Importez l’exemple de formulaire adaptatif.](./assets/bank-account-sample-form.zip)

* Spécifiez les valeurs appropriées dans la configuration du portail Azure à l’aide de la console de configuration OSGi.

* [Prévisualiser et soumettre le formulaire BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* Vérifiez que les données sont stockées dans le conteneur de stockage Azure de votre choix. Copiez l’ID d’objet blob.

* [Prévisualisez le formulaire BankAccount](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) et spécifiez l’ID d’objet blob comme paramètre guid dans l’URL pour que le formulaire soit prérempli avec les données provenant du stockage Azure.

