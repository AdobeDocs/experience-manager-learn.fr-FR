---
title: Utilisation de l’API usagerights
description: Exemple de code pour appliquer des droits d’utilisation au PDF fourni
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 3%

---

# Effectuer un appel API

## Appliquer les droits d’utilisation

Une fois que vous disposez du jeton d’accès, l’étape suivante consiste à effectuer une requête API pour appliquer des droits d’utilisation au PDF spécifié. Cela implique d’inclure le jeton d’accès dans l’en-tête de la requête pour authentifier l’appel, tout en assurant un traitement sécurisé et autorisé du document.

La fonction suivante applique les droits d’utilisation

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## Répartition des fonctions :



* **Configuration du point d’entrée de l’API et de la payload**
   * Construit l’URL de l’API à l’aide du `endPoint` fourni et d’un `BUCKET` prédéfini.
   * Définit une chaîne JSON (`usageRights`) spécifiant les droits à appliquer, tels que :
      * Commentaires
      * Fichiers incorporés
      * Remplir le formulaire
      * Exportation de données de formulaire

* **Charger le fichier PDF**
   * Récupère le fichier `withoutusagerights.pdf` dans le répertoire `pdffiles`.
   * Consigne une erreur et se ferme si le fichier est introuvable.

* **Préparer la requête HTTP**
   * Lit le fichier PDF dans un tableau d’octets.
   * Utilise `MultipartEntityBuilder` pour créer une requête en plusieurs parties contenant les éléments suivants :
      * Le fichier PDF en tant que corps binaire.
      * Le `usageRights` JSON comme corps de texte.
   * Configure une requête HTTP `POST` avec des en-têtes :
      * `Authorization: Bearer <accessToken>` pour l’authentification.
      * `X-Adobe-Accept-Experimental: 1` (éventuellement requis pour la compatibilité API).

* **Envoyer la requête et gérer la réponse**
   * Exécute la requête HTTP à l’aide de `httpClient.execute(httpPost)`.
   * Lit la réponse (attendue comme étant le PDF mis à jour avec les droits d’utilisation appliqués).
   * Écrit le contenu PDF reçu dans **« ReaderExtended.pdf »** sur `SAVE_LOCATION`.

* **Gestion et nettoyage des erreurs**
   * Capture et consigne toutes les erreurs `IOException`.
   * S’assure que toutes les ressources (flux, client HTTP, réponse) sont correctement fermées dans le bloc `finally`.

Voici le code main.java qui appelle la fonction applyUsageRights

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

La méthode `main` s’initialise en appelant `getAccessToken()` depuis `AccessTokenService`, qui doit renvoyer un jeton valide.

* Il appelle ensuite `applyUsageRights()` à partir de la classe `DocumentGeneration`, en transmettant :
   * L’`accessToken` récupéré
   * Point d’entrée de l’API pour l’application des droits d’utilisation.


## Étapes suivantes

[Déployer l’exemple de projet](sample-project.md)
