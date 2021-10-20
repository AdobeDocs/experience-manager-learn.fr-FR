---
title: Générer un document d’enregistrement interactif avec les données de formulaire adaptatif
description: Fusionner les données de formulaire adaptatif avec le modèle XDP pour générer un pdf téléchargeable
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
kt: 9226
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---


# Téléchargement d’un document d’enregistrement interactif

Un cas d’utilisation courant consiste à télécharger un document d’enregistrement interactif avec les données de formulaire adaptatif. Le document d’enregistrement téléchargé sera alors terminé à l’aide d’Adobe Acrobat ou d’Adobe Reader.

Pour réaliser ce cas pratique, procédez comme suit :

## Générer des exemples de données pour le XDP

* Ouvrez le fichier XDP dans AEM Forms Designer.
* Cliquez sur Fichier | Propriétés du formulaire | Aperçu
* Cliquez sur Générer les données d’aperçu
* Cliquez sur Générer
* Fournissez un nom de fichier significatif tel que &quot;form-data.xml&quot;.

## Générer un fichier XSD à partir des données xml

Vous pouvez utiliser n’importe quel outil en ligne gratuit pour [générer XSD](https://www.freeformatter.com/xsd-generator.html) des données xml générées à l’étape précédente.

## Créer un formulaire adaptatif

Créez un formulaire adaptatif basé sur le schéma XSD de l’étape précédente. Associez le formulaire pour utiliser la bibliothèque cliente &quot;irs&quot;. Cette bibliothèque cliente dispose du code pour effectuer un appel POST vers la servlet qui renvoie le PDF à l’application qui l’appelle. Le code suivant est déclenché lorsque la variable _PDF de téléchargement_ est cliqué

```javascript
$(document).ready(function() {
    $(".downloadpdf").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();

                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";

                var formData = new FormData();
                formData.append("dataXml", guideResultObject.data);
                console.log(guideResultObject.data);
                req.send(formData);

                req.onreadystatechange = function() {

                    if (req.readyState == 4 && req.status == 200) {



                        download(this.response, "report.pdf", "application/pdf");


                    }


                }
            }
        });

    });
});
```



## Création d’un servlet personnalisé

Créez un servlet personnalisé qui fusionnera les données avec le modèle XDP et renverra le pdf. Le code permettant d’y parvenir est répertorié ci-dessous. Le servlet personnalisé fait partie de la variable [Groupe AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringWriter;

import javax.servlet.Servlet;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerException;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Node;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = {
        Servlet.class
}, property = {
        "sling.servlet.methods=post",
        "sling.servlet.paths=/bin/generateinteractivedor"
})

public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
        @Reference
        DocumentServices documentServices;
        @Reference
        FormsService formsService;

        private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                // The xdp name can be passed to this servlet. For now it have been hardocded.

                String xdpName = "f8918-r14e_redo-barcode_3 2.xdp";

                XPathFactory xfact = XPathFactory.newInstance();
                XPath xpath = xfact.newXPath();
                //String dataXml = request.getParameter("formData");
                String dataXml = request.getParameter("dataXml");
                System.out.println("The data xml is " + dataXml);
                org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
                System.out.println("The af bound data is " + xmlDataDoc.getElementsByTagName("topmostSubform").getLength());
                try {
                        // get the actual xml data that needs to be merged with the template. This can be made more generic
                        Node res = (Node) xpath.evaluate("afData/afBoundData/topmostSubform", xmlDataDoc, XPathConstants.NODE);
                        StringWriter writer = new StringWriter();
                        Transformer transformer = TransformerFactory.newInstance().newTransformer();
                        transformer.transform(new DOMSource(res), new StreamResult(writer));
                        String xml = writer.toString();
                        System.out.println(xml);
                        xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                        Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                        String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                        com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                        renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                        renderOptions.setContentRoot(xdpTemplatePath);
                        renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                        Document xdpPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
                        InputStream fileInputStream = xdpPDF.getInputStream();
                        System.out.println("Got xdp PDF" + fileInputStream.available());
                        response.setContentType("application/pdf");
                        
                        response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
                        response.setContentLength((int) fileInputStream.available());
                        OutputStream responseOutputStream = response.getOutputStream();
                        int bytes;
                        while ((bytes = fileInputStream.read()) != -1) {
                                responseOutputStream.write(bytes);
                        }
                        responseOutputStream.flush();
                        responseOutputStream.close();

                } catch (XPathExpressionException e) {
                        log.debug(e.getMessage());

                } catch (TransformerException e) {

                        log.debug(e.getMessage());
                } catch (FormsServiceException e) {

                        log.debug(e.getMessage());
                } catch (IOException e) {

                        log.debug(e.getMessage());
                }

        }

}
```

Dans l’exemple de code, le nom du modèle (f8918-r14e_redo-barcode_3 2.xdp) est codé en dur. Vous pouvez facilement transmettre le nom du modèle au servlet pour rendre ce code générique afin qu’il fonctionne par rapport à tous les modèles.


## Déployer l’exemple sur votre serveur

Pour le tester sur votre serveur local, procédez comme suit :

1. [Télécharger et installer le bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Ajoutez l’entrée suivante dans le service Apache Sling User Mapper du service DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Télécharger et installer le lot Document Services personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Il dispose du servlet pour fusionner les données avec le modèle XDP et diffuser le pdf en continu.
1. [Importation de la bibliothèque cliente](assets/irs.zip)
1. [Importation du formulaire adaptatif](assets/f8918complete.zip)
1. [Importation du modèle et du schéma XDP](assets/xdp-template-and-xsd.zip)
1. [Aperçu du formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Remplir quelques champs de formulaire
1. Cliquez sur Télécharger le PDF pour obtenir le PDF. Vous devrez peut-être attendre quelques secondes pour que le PDF soit téléchargé.


