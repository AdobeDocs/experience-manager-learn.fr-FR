---
title: Stocker l’envoi du formulaire dans Azure Storage
description: Stockage des données de formulaire dans Azure Storage à l’aide de l’API REST
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Stocker les envois de formulaire dans Azure Storage

Cet article vous explique comment effectuer des appels REST pour stocker les données AEM Forms envoyées dans Azure Storage.
Pour pouvoir stocker les données de formulaire envoyées dans Azure Storage, les étapes suivantes doivent être suivies.

## Créer un compte Azure Storage

[Connectez-vous à votre compte Azure Portal et créez un compte de stockage.](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). Attribuez un nom significatif à votre compte de stockage, cliquez sur Réviser, puis sur Créer. Cela crée votre compte de stockage avec toutes les valeurs par défaut. Pour les besoins de cet article, nous avons nommé notre compte de stockage . `aemformstutorial`.

## Créer un accès partagé

Nous allons utiliser la signature d’accès partagé ou la méthode d’autorisation SAS pour interagir avec le conteneur de stockage Azure.
Sur la page du compte de stockage du portail, cliquez sur l’élément de menu Signature d’accès partagé à gauche pour ouvrir la nouvelle page des paramètres de clé de signature d’accès partagé. Veillez à définir les paramètres et la date de fin appropriée, comme indiqué dans la capture d&#39;écran ci-dessous, puis cliquez sur le bouton Générer SAS et chaîne de connexion . Copiez l’URL SAS de Blob Service. Nous utiliserons cette URL pour effectuer nos appels HTTP
![shared-access-keys](./assets/shared-access-signature.png)

## Créer un conteneur

Nous devons ensuite créer un conteneur pour stocker les données des envois de formulaire.
Sur la page du compte de stockage, cliquez sur l’option de menu Conteneurs située à gauche et créez un conteneur appelé `formssubmissions`. Assurez-vous que le niveau d’accès public est défini sur privé
![container](./assets/new-container.png)

## Créer une requête de PUT

L’étape suivante consiste à créer une demande de PUT pour stocker les données de formulaire envoyées dans Azure Storage. Nous devrons modifier l’URL du service Blob SAS pour inclure le nom du conteneur et l’ID BLOB dans l’URL. Chaque envoi de formulaire doit être identifié par un identifiant BLOB unique. L’identifiant BLOB unique est généralement créé dans votre code et inséré dans l’URL de la requête du PUT.
Voici l’URL partielle de la requête du PUT. La variable `aemformstutorial` est le nom du compte de stockage, formenvois est le conteneur dans lequel les données seront stockées avec un identifiant BLOB unique. Le reste de l’URL reste le même.
https://aemformstutorial.blob.core.windows.net/formsubmissions/00cb1a0c-a891-4dd7-9bd2-67a22bef3b8b?...............

La fonction suivante est écrite pour stocker les données de formulaire envoyées dans Azure Storage à l’aide d’une demande de PUT. Notez l’utilisation du nom du conteneur et de l’uuid dans l’URL. Vous pouvez créer un service OSGi ou un servlet Sling à l’aide de l’exemple de code répertorié ci-dessous et stocker les envois de formulaire dans Azure Storage.

```java
 public String saveFormDatainAzure(String formData) {
        System.out.println("in SaveFormData!!!!!"+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        
        String url = "https://aemformstutorial.blob.core.windows.net/formsubmissions/"+uuid.toString();
        url = url+"?sv=2022-11-02&ss=bf&srt=o&sp=rwdlaciytfx&se=2024-06-28T00:42:59Z&st=2023-06-27T16:42:59Z&spr=https&sig=v1MR%2FJuhEledioturDFRTd9e2fIDVSGJuAiUt6wNlkLA%3D";
        HttpPut httpPut = new HttpPut(url);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        try {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            log.debug("Response code "+response.getStatusLine().getStatusCode());
        } catch (IOException e) {
            log.error("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return uuid.toString();


    }
```

## Vérification des données stockées dans le conteneur

![form-data-in-container](./assets/form-data-in-container.png)
