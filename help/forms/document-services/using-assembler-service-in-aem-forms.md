---
title: Utilisation du service Assembler dans AEM Forms
seo-title: Utilisation du service Assembler dans AEM Forms
description: Utilisation du service Assembler dans AEM Forms pour assembler plusieurs fichiers pdf
seo-description: Utilisation du service Assembler dans AEM Forms pour assembler plusieurs fichiers pdf
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: Assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 5%

---


# Utilisation du service Assembler dans AEM Forms{#using-assembler-service-in-aem-forms}

Cet article vous fournit les ressources qui vous permettent de faire glisser et de déposer plusieurs fichiers PDF dans le navigateur et d’enregistrer le fichier pdf assemblé dans votre système de fichiers. Voici le code du servlet qui assemble les fichiers pdf téléchargés à l’aide du navigateur.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

Pour que cette fonctionnalité fonctionne sur votre serveur AEM

* Téléchargez le fichier [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) sur votre système local.
* Téléchargez et installez le module à l’aide du [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp).
* Télécharger[Groupe de services de document personnalisés](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Télécharger [Développement avec le lot utilisateur du service](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Déployez et démarrez les lots à l’aide de la [console web felix](http://localhost:4502/system/console/bundles)
* Pointez votre navigateur sur [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* Glisser-déposer quelques fichiers de fichiers PDF

>[!NOTE]
>
>Assurez-vous que votre installation d’AEM Forms est terminée. Tous vos lots doivent être en principal état.
>
>Assurez-vous d’avoir ajouté : déléguez le démarrage des bibliothèques RSA et BouncyCastle comme mentionné dans cette [installation d’AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**Avertissements pour cette démonstration**
>
> * Le code ne prend pas en charge les documents PDF basés sur XFA.
   >
   > 
* Veillez à ne faire glisser que des fichiers PDF.
>
>







