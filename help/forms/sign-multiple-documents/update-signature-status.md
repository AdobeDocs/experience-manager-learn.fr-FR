---
title: Mettre à jour l’état de signature du formulaire dans la base de données
description: Mettre à jour l’état de signature du formulaire signé dans la base de données à l’aide du workflow AEM
feature: Adaptive Forms
version: 6.4,6.5
kt: 6888
thumbnail: 6888.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 75852a4b-7008-4c65-bab1-cc5dbf525e20
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 2%

---

# Mettre à jour l’état de signature

Le workflow UpdateSignatureStatus est déclenché lorsque l’utilisateur a terminé la cérémonie de signature. Voici le flux du workflow :

![main-workflow](assets/update-signature.PNG)

Mettre à jour le statut de la signature est une étape de processus personnalisée.
La principale raison de l’implémentation de l’étape de processus personnalisé est d’étendre un processus AEM. Voici le code personnalisé utilisé pour mettre à jour l’état de la signature.
Le code de cette étape de processus personnalisée référence le service SignMultipleForms.


```java
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Update Signature Status in DB",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Update Signature Status in DB"
})

public class UpdateSignatureStatusWorkflowStep implements WorkflowProcess {
  private static final Logger log = LoggerFactory.getLogger(UpdateSignatureStatusWorkflowStep.class);@Reference
  SignMultipleForms signMultipleForms;@Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap args) throws WorkflowException {
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFilePath = payloadPath + "/Data.xml/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    DocumentBuilderFactory factory = null;
    DocumentBuilder builder = null;
    Document xmlDocument = null;
    Node xmlDataNode = null;
    try {
      xmlDataNode = session.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      factory = DocumentBuilderFactory.newInstance();
      builder = factory.newDocumentBuilder();
      xmlDocument = builder.parse(xmlDataStream);
      XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
      org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afUnboundData/data/guid").evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
      String guid = node.getTextContent();
      StringWriter writer = new StringWriter();
      IOUtils.copy(xmlDataStream, writer, StandardCharsets.UTF_8);
      System.out.println("After ioutils copy" + writer.toString());
      signMultipleForms.updateSignatureStatus(writer.toString(), guid);
    }
    catch(Exception e) {
      log.debug(e.getMessage());
    }

  }

}
```

## Ressources

Le processus de mise à jour de l’état de la signature peut être [téléchargé ici](assets/update-signature-status-workflow.zip)

## Étapes suivantes

[Personnalisation de l’étape de résumé pour afficher le formulaire suivant à signer](./customize-summary-component.md)
