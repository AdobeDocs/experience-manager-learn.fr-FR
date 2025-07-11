---
title: Assembler les fichiers PDF à l’aide d’une opération DDX
description: Effectuer une requête POST pour appeler le point d’entrée DDX avec les paramètres nécessaires
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 693dac88-84f3-4051-8e46-3105093711a3
duration: 56
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '124'
ht-degree: 100%

---

# Effectuer l’appel POST


L’étape suivante consiste à effectuer un appel POST HTTP au point d’entrée avec les paramètres nécessaires. Le code DDX et les fichiers PDF sont disponibles en tant que fichiers de ressources. Le point d’entrée dispose d’une authentification par jeton. Le jeton d’accès est transmis dans l’en-tête de la requête.
Le service Assembler utilise un langage de type XML, appelé Document Description XML (DDX), avec lequel vous pouvez décrire la sortie que vous souhaitez obtenir. DDX est un langage de balisage déclaratif dont les éléments représentent des blocs de création de documents. Le code DDX suivant permet de fusionner les deux documents PDF identifiés dans les éléments source du PDF.

```xml
<DDX xmlns="http://ns.adobe.com/DDX/1.0/">
<PDF result="doc3.pdf"> 
	<PDF source="CA-Drivers-Handbook.pdf"/>
 	<PDF source="CA-Parent-Teen-Handbook.pdf"/>
  </PDF>
</DDX>
```

Le code suivant permet de combiner les fichiers pdf.

```java
package com.aemformscs.documentservices;

import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.net.URL;

import org.apache.commons.io.IOUtils;
import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.HttpMultipartMode;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;

public class AssemblePDFFiles {
  public String SAVE_LOCATION = "c:\\assembledpdf";

  public void assemblePDF(String postURL) {

    HttpPost httpPost = new HttpPost(postURL);
    CredentialUtilites cu = new CredentialUtilites();
    String accessToken = cu.getAccessToken();
    httpPost.addHeader("Authorization", "Bearer " + accessToken);
    ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
    URL ddxFile = classLoader.getResource("ddxfiles/assemble2pdfs.ddx");
    MultipartEntityBuilder builder = MultipartEntityBuilder.create();

    builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);

    File ddx = new File(ddxFile.getPath());
    builder.addBinaryBody("ddx", ddx, ContentType.create("application/xml"), ddx.getName());
    URL url = classLoader.getResource("pdffiles");
    System.out.println(url.getPath());
    File files[] = new File(url.getPath()).listFiles();

    for (int i = 0; i < files.length; i++) {
      System.out.println("Added  " + files[i].getName());
      builder.addBinaryBody(files[i].getName(), files[i], ContentType.APPLICATION_OCTET_STREAM, files[i].getName());
    }
    try {

      HttpEntity entity = builder.build();
      httpPost.setEntity(entity);
      CloseableHttpClient httpclient = HttpClients.createDefault();
      CloseableHttpResponse response = httpclient.execute(httpPost);
      System.out.println("The success code is " + response.getStatusLine().getStatusCode());
      InputStream generatedPDF = response.getEntity().getContent();
      byte[] bytes = IOUtils.toByteArray(generatedPDF);
      File saveLocation = new File(SAVE_LOCATION);
      if (!saveLocation.exists()) {
        saveLocation.mkdirs();
      }
      File outputFile = new File(SAVE_LOCATION + File.separator + "assembledPDF.pdf");
      FileOutputStream outputStream = new FileOutputStream(outputFile);
      outputStream.write(bytes);
      outputStream.close();
      System.out.println("AssembledPDF.pdf saved to " + SAVE_LOCATION);

    } catch (Exception e) {
      System.out.println("The message is " + e.getMessage());
    }
  }

}
```
