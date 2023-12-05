---
title: Appliquer les droits d’utilisation au PDF chargé
description: Appliquer des droits d’utilisation aux PDF
version: 6.4,6.5
feature: Reader Extensions
topic: Development
role: Developer
level: Experienced
exl-id: ea433667-81db-40f7-870d-b16630128871
last-substantial-update: 2020-07-07T00:00:00Z
duration: 193
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 100%

---

# Appliquer Reader Extensions

Les Reader Extensions vous permettent de manipuler les droits d’utilisation des documents PDF. Les droits d’utilisation se rapportent aux fonctionnalités disponibles dans Acrobat mais pas dans Adobe Reader. Les fonctionnalités contrôlées par Reader Extensions permettent d’ajouter des commentaires à un document, de remplir des formulaires et d’enregistrer le document. Les documents PDF dotés de droits d’utilisation sont appelés des documents dont les droits sont activés. Un utilisateur qui ouvre un document PDF dont les droits sont activés dans Adobe Reader peut effectuer les opérations qui sont autorisées pour ce document.

Pour réaliser ce cas d’utilisation, procédez comme suit :
* [Ajoutez le certificat Reader Extensions](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=fr) à l’utilisateur ou à l’utilisatrice `fd-service`.

## Créez un service OSGi personnalisé.

Créez un service OSGi personnalisé qui appliquera des droits d’utilisation aux documents. Le code permettant d’y parvenir est fourni ci-dessous.

```java
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = ApplyUsageRights.class)
public class ApplyUsageRights implements ReaderExtendPDF {
        @Reference
        DocAssuranceService docAssuranceService;
        @Reference
        GetResolver getResolver;
        Logger logger = LoggerFactory.getLogger(ApplyUsageRights.class);
        @Override
        public Document applyUsageRights(Document pdfDocument, UsageRights usageRights) {

                ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
                UnlockOptions unlockOptions = null;
                ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
                reOptions.setCredentialAlias("ares");

                reOptions.setResourceResolver(getResolver.getFormsServiceResolver());

                reOptions.setReOptions(reOptionsSpec);
                System.out.println("Applying Usage Rights");

                try {
                        Document readerExtended = docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
                                unlockOptions);
                        reOptions.getResourceResolver().close();
                        return readerExtended;
                } catch (Exception e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                return null;
        }

}
```

## Créez un servlet pour diffuser le PDF étendu grâce à Reader Extensions.

Vous pouvez également créer un servlet avec une méthode POST pour renvoyer le PDF étendu grâce à Reader Extensions à l’utilisateur ou à l’utilisatrice. Dans ce cas, la personne utilisatrice est invitée à enregistrer le PDF dans son système de fichiers. Cela est dû au fait que le PDF est rendu en tant que PDF dynamique et que les visionneuses PDF fournies avec les navigateurs ne prennent pas en charge les PDF dynamiques.

Voici le code pour le servlet. Le servlet est appelé à partir de l’action customsubmit du formulaire adaptatif.
Le servlet crée l’objet UsageRights et définit ses propriétés en fonction des valeurs saisies par l’utilisateur ou l’utilisatrice dans le formulaire adaptatif. Le servlet appelle ensuite la méthode applyUsageRights du service créé à cet effet.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        private static final long serialVersionUID = -883724052368090823 L;
        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];

                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got form attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);
                                        if (logger.isDebugEnabled()) {
                                                documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        }

                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();

                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=" + param.getFileName().split("/")[1]);
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        documentToReaderExtend.close();
                                        int bytes;
                                        while ((bytes = fileInputStream.read()) != -1) {
                                                responseOutputStream.write(bytes);
                                        }
                                        responseOutputStream.flush();
                                        responseOutputStream.close();

                                } catch (IOException e) {
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

Pour tester ceci sur votre serveur local, procédez comme suit :
1. [Télécharger et installer le lot DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Téléchargez et installez le lot ares.ares.core-ares.](assets/ares.ares.core-ares.jar). Il dispose du service personnalisé et du servlet pour appliquer des droits d’utilisation et renvoyer le PDF.
1. [Importer les bibliothèques clientes et personnaliser l’envoi](assets/applyaresdemo.zip)
1. [Importer le formulaire adaptatif](assets/applyaresform.zip)
1. Ajoutez le certificat Reader Extensions à l’utilisateur ou à l’utilisatrice « fd-service ». Assurez-vous que l’alias est bien « ares ».
1. [Prévisualiser le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Sélectionnez les droits appropriés et chargez le fichier PDF.
1. Cliquez sur Envoyer pour obtenir le PDF étendu grâce à Reader Extensions.
