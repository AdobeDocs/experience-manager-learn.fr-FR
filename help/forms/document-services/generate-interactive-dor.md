---
title: Générer un document d’enregistrement interactif avec les données de formulaire adaptatif
description: Fusionner les données de formulaire adaptatif avec le modèle XDP pour générer un PDF téléchargeable
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 100%

---

# Télécharger un document d’enregistrement interactif

Un cas d’utilisation courant consiste à télécharger un document d’enregistrement interactif avec les données de formulaire adaptatif. Le document d’enregistrement téléchargé sera alors terminé à l’aide d’Adobe Acrobat ou d’Adobe Reader.

## Le formulaire adaptatif n’est pas basé sur un schéma XSD

Si votre XDP et votre formulaire adaptatif ne sont basés sur aucun schéma, procédez comme suit pour générer un document d’enregistrement interactif.

### Créer un formulaire adaptatif

Créez un formulaire adaptatif et assurez-vous que les champs du formulaire adaptatif portent les mêmes noms que ceux des champs dans votre modèle XDP.
Notez le nom de l’élément racine de votre modèle XDP.
![root-element](assets/xfa-root-element.png)

### Bibliothèque cliente

Le code suivant est déclenché lorsque le bouton Télécharger le PDF est déclenché.

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
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

## Formulaire adaptatif basé sur un schéma XSD

Si votre XDP n’est pas basé sur XSD, procédez comme suit pour créer un schéma XSD sur lequel vous baserez votre formulaire adaptatif.

### Générer des données d’exemple pour le XDP

* Ouvrez le fichier XDP dans AEM Forms Designer.
* Cliquez sur Fichier | Propriétés du formulaire | Aperçu.
* Cliquez sur Générer les données d’aperçu.
* Cliquez sur Générer.
* Fournissez un nom de fichier significatif, tel que « form-data.xml ».

### Générer un fichier XSD à partir de données XML

Vous pouvez utiliser n’importe quel outil en ligne gratuit pour [générer le XSD](https://www.freeformatter.com/xsd-generator.html) à partir des données xml générées à l’étape précédente.

### Créer un formulaire adaptatif

Créez un formulaire adaptatif basé sur le schéma XSD de l’étape précédente. Associez le formulaire pour utiliser la bibliothèque cliente « irs ». Cette bibliothèque cliente dispose du code pour effectuer un appel POST vers le servlet qui renvoie le PDF à l’application qui l’appelle.
Le code suivant est déclenché lorsque l’on clique sur _Télécharger le PDF_.

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
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



## Créer un servlet personnalisé

Créez un servlet personnalisé qui fusionnera les données avec le modèle XDP et renverra le PDF. Le code permettant de réaliser cette opération figure ci-dessous. Le servlet personnalisé fait partie du [lot AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
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

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

Dans l’exemple de code, nous extrayons le nom XDP et d’autres paramètres de l’objet de la requête. Si le formulaire n’est pas basé sur XSD, le document XML à fusionner avec XDP est créé. Si le formulaire est basé sur XSD, nous extrayons simplement le nœud approprié à partir des données du formulaire adaptatif envoyé pour générer un document XML à fusionner avec le modèle XDP.

## Déployer l’exemple sur votre serveur

Pour tester ceci sur votre serveur local, procédez comme suit :

1. [Télécharger et installer le lot DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Ajoutez l’entrée suivante dans le service de mappage utilisateur ou utilisatrice de service Apache Sling.
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [Téléchargez et installez le lot DocumentServices personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Il dispose du servlet pour fusionner les données avec le modèle XDP et renvoyer le PDF.
1. [Importer la bibliothèque cliente](assets/generate-interactive-dor-client-lib.zip)
1. [Importez les ressources de l’article (formulaire adaptatif, modèles XDP et schéma XSD).](assets/generate-interactive-dor-sample-assets.zip)
1. [Prévisualisez le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled).
1. Renseignez quelques champs du formulaire.
1. Cliquez sur Télécharger le PDF pour obtenir le PDF. Vous devrez peut-être attendre quelques secondes pour que le PDF soit téléchargé.

>[!NOTE]
>
>Vous pouvez essayer le même cas d’utilisation avec un [formulaire adaptatif non basé sur XSD](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Veillez à transmettre les paramètres appropriés au point d’entrée POST dans streampdf.js situé dans la bibliothèque cliente IRS.
