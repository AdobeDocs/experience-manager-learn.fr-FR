---
title: Créer un service OSGi pour exporter des données à partir d’un formulaire PDF
description: Exporter les données d’un formulaire PDF à l’aide de l’API FormsService
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
exl-id: c3032669-154c-4565-af6e-32d94e975e37
source-git-commit: 7a0ec4797fda0436a8c20b84d1e36a8d16af21b9
workflow-type: ht
source-wordcount: '135'
ht-degree: 100%

---

# Exporter des données

La première étape pour renseigner un formulaire adaptatif à partir d’un fichier PDF consiste à exporter les données du fichier PDF et à les stocker dans le référentiel AEM.

Le code suivant permet d’extraire les données du PDF chargé et de les transmettre dans un format correct pour remplir le formulaire adaptatif.

```java
public String getFormData(Document pdfForm) {
   DocumentBuilderFactory factory = null;
   DocumentBuilder builder = null;
   org.w3c.dom.Document xmlDocument = null;

   try {
      Document xmlData = formsService.exportData(pdfForm, DataFormat.Auto);
      xmlData.copyToFile(new File("xmlData.xml"));
      factory = DocumentBuilderFactory.newInstance();
      factory.setNamespaceAware(true);
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlData.getInputStream());

      org.w3c.dom.Node xdpNode = xmlDocument.getDocumentElement();
      log.debug("Got xdp " + xdpNode.getNodeName());
      org.w3c.dom.Node datasets = getChildByTagName(xdpNode, "xfa:datasets");
      log.debug("Got datasets " + datasets.getNodeName());
      org.w3c.dom.Node data = getChildByTagName(datasets, "xfa:data");
      log.debug("Got data " + data.getNodeName());
      org.w3c.dom.Node topmostSubform = getChildByTagName(data, "topmostSubform");

      if (topmostSubform != null) {

         org.w3c.dom.Document newXmlDocument = builder.newDocument();
         org.w3c.dom.Node importedNode = newXmlDocument.importNode(topmostSubform, true);
         newXmlDocument.appendChild(importedNode);
         Document aemFDXmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXmlDocument);
         aemFDXmlDocument.copyToFile(new File("aemFDXmlDocument.xml"));
         // saveDocumentInCrx is an utility method of the custom DocumentServices service. 
         return documentServices.saveDocumentInCrx("/content/exporteddata", ".xml", aemFDXmlDocument);
      }

   } catch (Exception e) {
      log.debug("Error:  " + e.getMessage());

   }
   return null;
}
```

Voici la fonction utilitaire écrite pour extraire _**topmostSubForm**_ avec les espaces de noms appropriés.

```java
private static org.w3c.dom.Node getChildByTagName(org.w3c.dom.Node parent, String tagName) {
   NodeList nodeList = parent.getChildNodes();
   for (int i = 0; i < nodeList.getLength(); i++) {
      org.w3c.dom.Node node = nodeList.item(i);
      if (node.getNodeType() == org.w3c.dom.Node.ELEMENT_NODE && node.getNodeName().equals(tagName)) {
         return node;
      }
   }
   return null;
}
```

Les données extraites sont stockées sous le nœud /content/exporteddata dans le référentiel CRX. Le chemin d’accès au fichier des données exportées est alors renvoyé à l’application qui appelle pour renseigner le formulaire adaptatif.

## Étapes suivantes

[Importer des données d’un fichier PDF](./populate-adaptive-form.md)
