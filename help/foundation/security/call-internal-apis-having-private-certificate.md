---
title: Appel d’API internes avec des certificats privés
description: Découvrez comment appeler des API internes comportant des certificats privés ou auto-signés.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
kt: 11548
thumbnail: KT-11548.png
doc-type: article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
source-git-commit: 68aaa58c8f95e72e1a7cb89f849c77d1210f31ee
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Appel d’API internes avec des certificats privés

Découvrez comment effectuer des appels HTTPS d’AEM aux API web à l’aide de certificats privés ou auto-signés.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

Par défaut, lorsque vous tentez d’établir une connexion HTTPS à une API web qui utilise un certificat auto-signé, la connexion échoue avec l’erreur :

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

Ce problème survient généralement lorsque la variable **Le certificat SSL de l’API n’est pas émis par une autorité de certification reconnue** et l’application Java™ ne peut pas valider le certificat SSL/TLS.

Découvrez comment appeler avec succès des API qui possèdent des certificats privés ou auto-signés à l’aide de [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) et **AEM TrustStore global**.


## Prototype du code d’appel d’API à l’aide de HttpClient

Le code suivant établit une connexion HTTPS à une API web :

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

Le code utilise la variable [Apache HttpComponent](https://hc.apache.org/)&#39;s [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) classes de bibliothèque et leurs méthodes.


## HttpClient et chargez AEM matériel TrustStore

Pour appeler un point de terminaison d’API qui possède _certificat privé ou auto-signé_, la variable [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)&#39;s `SSLContextBuilder` doit être chargé avec AEM TrustStore et utilisé pour faciliter la connexion.

Suivez les étapes ci-dessous :

1. Connexion à **Auteur AEM** as a **administrator**.
1. Accédez à **AEM Auteur > Outils > Sécurité > Trust Store**, puis ouvrez le **Trust Store mondial**. Si vous accédez pour la première fois, définissez un mot de passe pour le Trust Store global.

   ![TrustStore global](assets/internal-api-call/global-trust-store.png)

1. Pour importer un certificat privé, cliquez sur **Sélectionner le fichier de certificat** et sélectionnez le fichier de certificat souhaité avec `.cer` extension . Importer en cliquant sur **Envoyer** bouton .

1. Mettez à jour le code Java™ comme ci-dessous. Notez que pour utiliser `@Reference` pour obtenir des AEM `KeyStoreService` le code d’appel doit être un composant/service OSGi ou un modèle Sling (et `@OsgiService` y est utilisé).

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * Injection de la OOTB `com.adobe.granite.keystore.KeyStoreService` Service OSGi dans votre composant OSGi.
   * Obtenez l’AEM globale de TrustStore à l’aide de `KeyStoreService` et `ResourceResolver`, la variable `getAEMTrustStore(...)` fait cela.
   * Créez un objet de `SSLContextBuilder`, voir Java™ [Détails de l’API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
   * Chargez le TrustStore d’AEM global dans `SSLContextBuilder` using `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` .
   * Pass `null` pour `TrustStrategy` dans la méthode ci-dessus, elle garantit que seuls AEM certificats approuvés réussissent lors de l’exécution de l’API.


>[!CAUTION]
>
>Les appels d’API avec des certificats valides délivrés par une autorité de certification échouent lors de l’exécution de l’approche mentionnée. Seuls les appels d’API avec AEM certificats approuvés sont autorisés à réussir en suivant cette méthode.
>
>Utilisez la variable [approche standard](#prototypical-api-invocation-code-using-httpclient) pour exécuter des appels d’API de certificats valides délivrés par une autorité de certification, ce qui signifie que seules les API associées à des certificats privés doivent être exécutées à l’aide de la méthode mentionnée précédemment.

## Éviter les modifications du KeyStore JVM

Pour appeler efficacement les API internes avec des certificats privés, une approche conventionnelle consiste à modifier le KeyStore JVM. Pour ce faire, vous devez importer les certificats privés à l’aide de Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) .

Toutefois, cette méthode n’est pas alignée sur les bonnes pratiques en matière de sécurité et AEM offre une option supérieure grâce à l’utilisation de la variable **Trust Store mondial** et [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).


## Package de solution

L’exemple de projet Node.js illustré dans la vidéo peut être téléchargé à partir de [here](assets/internal-api-call/REST-APIs.zip).

Le code du servlet AEM est disponible dans le projet WKND Sites `tutorial/web-api-invocation` branche, [see](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
