---
title: Authentification Transport Layer Security mutuelle (mTLS) à partir d’AEM
description: Découvrez comment effectuer des appels HTTPS à partir d’AEM vers les API web qui nécessitent une authentification Transport Layer Security mutuelle (mTLS).
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-13881
thumbnail: KT-13881.png
doc-type: Article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
duration: 524
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 100%

---

# Authentification Transport Layer Security mutuelle (mTLS) à partir d’AEM

Découvrez comment effectuer des appels HTTPS à partir d’AEM vers les API web qui nécessitent une authentification Transport Layer Security mutuelle (mTLS).

>[!VIDEO](https://video.tv.adobe.com/v/3424855?quality=12&learn=on)

L’authentification mTLS ou TLS bidirectionnelle renforce la sécurité du protocole TLS en exigeant une **authentification mutuelle entre le client ou la cliente et le serveur**. Cette authentification est effectuée à l’aide de certificats numériques. Elle est généralement utilisée dans les cas où une sécurité renforcée et une vérification de l’identité sont essentielles.

Par défaut, lorsque vous tentez d’établir une connexion HTTPS à une API web nécessitant une authentification mTLS, la connexion échoue avec l’erreur :

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

Ce problème se produit lorsque le client ou la cliente ne présente pas de certificat pour s’authentifier.

Découvrez comment appeler avec succès des API qui nécessitent une authentification mTLS à l’aide d’[Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) et du **KeyStore et TrustStore d’AEM**.


## HttpClient et charger le matériel KeyStore d’AEM

À un niveau élevé, les étapes suivantes sont requises pour appeler une API protégée par mTLS à partir d’AEM.

### Génération de certificats AEM

Demandez le certificat AEM en vous associant à l’équipe de sécurité de votre entreprise. L’équipe de sécurité fournit ou demande les informations relatives au certificat, telles que la clé ou la demande de signature de certificat (CSR). Le certificat est émis à l’aide de la CSR.

À des fins de démonstration, générez les détails liés au certificat, tels que la clé, la demande de signature de certificat (CSR). Dans l’exemple ci-dessous, une autorité de certification auto-signée est utilisée pour émettre le certificat.

- Tout d’abord, générez le certificat de l’autorité de certification (CA) interne.

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- Générez le certificat AEM.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- Convertissez la clé privée AEM au format DER. Le KeyStore d’AEM nécessite la clé privée au format DER.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>Les certificats auto-signés de l’autorité de certification sont utilisés à des fins de développement uniquement. Pour la production, utilisez une autorité de certification approuvée pour émettre le certificat.


### Échange de certificats

Si vous utilisez une autorité de certification auto-signée pour le certificat AEM, comme ci-dessus, envoyez le certificat ou le certificat de l’autorité de certification interne au fournisseur d’API.

En outre, si le fournisseur d’API utilise un certificat d’autorité de certification auto-signé, vous recevez le certificat ou le certificat de l’autorité de certification interne du fournisseur d’API.

### Import de certificat

Pour importer le certificat AEM, procédez comme suit :

1. Connectez-vous à l’**instance de création AEM** en tant qu’**administrateur ou administratrice**.

1. Accédez à **Instance de création AEM > Outils > Sécurité > Utilisateurs > Créer ou sélectionner un utilisateur existant**.

   ![Création ou sélection d’un utilisateur existant ou d’une utilisatrice existante.](assets/mutual-tls-authentication/create-or-select-user.png)

   À des fins de démonstration, une nouvelle personne nommée `mtl-demo-user` est créée.

1. Pour ouvrir les **Propriétés**, cliquez sur le nom.

1. Cliquez sur l’onglet **KeyStore**, puis cliquez sur le bouton **Créer un KeyStore**. Ensuite, dans la boîte de dialogue **Définir le mot de passe d’accès au KeyStore**, définissez un mot de passe pour le KeyStore de cette personne, puis cliquez sur Enregistrer.

   ![Créer le KeyStore](assets/mutual-tls-authentication/create-keystore.png)

1. Dans le nouvel écran, sous la section **Ajouter une clé privée à partir du fichier DER**, procédez comme suit :

   1. Saisissez l’alias.

   1. Importez la clé privée AEM au format DER, générée ci-dessus.

   1. Importez les fichiers de chaîne de certificats, générés ci-dessus.

   1. Cliquez sur Envoyer

      ![Import d’une clé privée AEM.](assets/mutual-tls-authentication/import-aem-private-key.png)

1. Vérifiez que le certificat a bien été importé.

   ![Clé privée AEM et certificat importé.](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

Si le fournisseur d’API utilise un certificat d’autorité de certification auto-signé, importez le certificat reçu dans le TrustStore d’AEM, et suivez les étapes [ici](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html?lang=fr).

De même, si AEM utilise un certificat auto-signé de l’autorité de certification, demandez au fournisseur d’API de l’importer.

### Prototype du code d’appel de l’API mTLS à l’aide de HttpClient

Mettez à jour le code Java™ comme ci-dessous. Pour utiliser l’annotation `@Reference` afin d’obtenir le service `KeyStoreService` d’AEM, le code d’appel doit être un composant/service OSGi ou un modèle Sling (et `@OsgiService` y est utilisé).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- Injectez le service OSGi `com.adobe.granite.keystore.KeyStoreService` prêt à l’emploi dans votre composant OSGi.
- Procurez-vous le KeyStore AEM de la personne à l’aide de `KeyStoreService` et `ResourceResolver`. C’est ce que fait la méthode `getAEMKeyStore(...)`.
- Si le fournisseur d’API utilise un certificat auto-signé d’autorité de certification, obtenez le TrustStore global d’AEM. C’est ce que fait la méthode `getAEMTrustStore(...)`.
- Créez un objet de `SSLContextBuilder`, voir les [détails de l’API](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html) Java™.
- Chargez le KeyStore AEM de la personne dans `SSLContextBuilder` à l’aide de la méthode `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)`.
- Le mot de passe du KeyStore est le mot de passe qui a été défini lors de la création du KeyStore. Il doit être stocké dans la configuration OSGi. Voir [Valeurs de configuration du secret](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=fr#secret-configuration-values).

## Éviter les modifications du KeyStore JVM

Pour appeler efficacement les API mTLS avec des certificats privés, une approche conventionnelle consiste à modifier le KeyStore JVM. Pour ce faire, vous devez importer les certificats privés à l’aide de la commande [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) Java™.

Toutefois, cette méthode n’est pas alignée sur les bonnes pratiques en matière de sécurité et AEM offre une option supérieure grâce à l’utilisation des **KeyStores spécifiques à la personne et du TrustStore global** et de [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).

## Package de solution

L’exemple de projet Node.js illustré dans la vidéo peut être téléchargé [ici](assets/internal-api-call/REST-APIs.zip).

Le code du servlet AEM est disponible dans la branche `tutorial/web-api-invocation` de projet de sites WKND, [voir ici](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
