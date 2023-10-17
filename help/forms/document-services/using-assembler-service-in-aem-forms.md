---
title: Utiliser le service Assembler dans AEM Forms
description: Utiliser le service Assembler dans AEM Forms pour assembler plusieurs fichiers PDF
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '207'
ht-degree: 100%

---

# Utiliser le service Assembler dans AEM Forms{#using-assembler-service-in-aem-forms}

Cet article vous fournit les ressources pour illuster la fonctionalité de faire glisser-déposer plusieurs fichiers PDF dans le navigateur et d’enregistrer le fichier PDF assemblé dans votre système de fichiers. Voici le code du servlet qui assemble les fichiers PDF téléchargés à l’aide du navigateur.

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

Pour que cette fonctionnalité soit valide sur votre serveur AEM :

* Téléchargez [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) dans votre système local.
* Chargez et installez le package à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Téléchargez le [lot Document Services personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).
* Téléchargez le [lot Developing with Service User](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Déployez et démarrez les lots à l’aide de la [console web felix](http://localhost:4502/system/console/bundles).
* Pointez votre navigateur sur [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html).
* Faites glisser et déposez quelques fichiers PDF.

>[!NOTE]
>
>Assurez-vous que votre installation d’AEM Forms est terminée. Tous vos lots doivent être à l’état actif.
>
>Assurez-vous d’avoir ajouté les bibliothèques Boot delegate RSA et BouncyCastle comme indiqué dans la section [Installation d’AEM Forms](https://helpx.adobe.com/fr/aem-forms/6-3/installing-configuring-aem-forms-osgi.html).
>
>**Avertissements pour cette démonstration**
>
> * Le code ne gère pas les documents PDF basés sur XFA.
>
> * Veillez à ne faire glisser et déposer que des fichiers PDF.
>
>
