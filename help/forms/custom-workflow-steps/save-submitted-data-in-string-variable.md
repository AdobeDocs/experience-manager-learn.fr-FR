---
title: Enregistrer les données envoyées dans la variable string
description: Étape du processus personnalisée pour extraire les données liées et les enregistrer dans une variable de workflow de type chaîne
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11199
last-substantial-update: 2022-10-02T00:00:00Z
thumbnail: string-variable.jpg
exl-id: 65dcbfbb-7eb5-4fa3-aeb3-587c59ee2fe9
duration: 74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '158'
ht-degree: 100%

---

# Extraire des données liées et les enregistrer dans une variable de chaîne

Cette fonctionnalité vous permet d’inclure les données envoyées dans le corps de votre e-mail. L’étape de processus personnalisée extrait les **données liées** à partir de l’envoi du formulaire adaptatif et renseigne une variable de type chaîne avec les données. Vous pouvez ensuite utiliser cette variable de chaîne pour insérer les données dans votre modèle d’e-mail.
La copie d’écran suivante montre les arguments que vous devez transmettre à l’étape de processus personnalisée.
![process-step](assets/save-submitted-data-string.png)

Voici les paramètres :

* `data.xml` : le fichier contenant les données envoyées. Si le format est json, le nom du fichier peut être data.json.

L’étape de processus personnalisée extrait ensuite les données liées et les stocke dans la variable submittedDataString définie dans le workflow.


[Le lot personnalisé peut être téléchargé ici.](assets/AEMFormsProcessStep.core-1.0.0-SNAPSHOT.jar)

```java
package AEMFormsProcessStep.core;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import javax.jcr.Session;
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathFactory;
import org.w3c.dom.Document;
import javax.xml.parsers.DocumentBuilder;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.StringWriter;

import javax.jcr.Node;

@Component(property = {
  "service.description=Save submitted Payload as String",
  "process.label=Save submitted Payload as String"
})
public class SaveSubmittedDataInStringVariable implements WorkflowProcess {
  private Logger log;

  @Override
  public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metaDataMap) throws WorkflowException {

    String submittedDataFile = ((String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string")).toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    log = LoggerFactory.getLogger(SaveSubmittedDataInStringVariable.class);

    String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
    Session session = (Session) workflowSession.adaptTo((Class) Session.class);
    Node submittedDataNode = null;
    try {
      submittedDataNode = session.getNode(dataFilePath);
      if (submittedDataFile.endsWith("json")) {
        InputStream jsonDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
        BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        while ((inputStr = streamReader.readLine()) != null) {
          responseStrBuilder.append(inputStr);
        }
        JsonObject jsonObject = new Gson().fromJson(responseStrBuilder.toString(), JsonObject.class);
        JsonObject boundData = jsonObject.getAsJsonObject("afData").getAsJsonObject("afBoundData").getAsJsonObject("data");
        System.out.println(jsonObject.getAsJsonObject("afData").getAsJsonObject("afBoundData").getAsJsonObject("data"));
        metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
        metaDataMap.put("submittedDataString", boundData.toString());

      }
      if (submittedDataFile.endsWith("xml")) {
        log.debug("Got xml file");
        DocumentBuilderFactory factory = null;
        DocumentBuilder builder = null;
        Document xmlDocument = null;
        InputStream xmlDataStream = submittedDataNode.getProperty("jcr:data").getBinary().getStream();
        System.out.println("Got xml file" + submittedDataNode);
        XPath xPath = XPathFactory.newInstance().newXPath();
        factory = DocumentBuilderFactory.newInstance();
        builder = factory.newDocumentBuilder();
        xmlDocument = builder.parse(xmlDataStream);

        org.w3c.dom.Node node = (org.w3c.dom.Node) xPath.compile("/afData/afBoundData").evaluate(xmlDocument, XPathConstants.NODE);
        Transformer transformer = TransformerFactory.newInstance().newTransformer();
        transformer.setOutputProperty(OutputKeys.INDENT, "yes");
        StreamResult result = new StreamResult(new StringWriter());
        DOMSource source = new DOMSource(node);
        transformer.transform(source, result);
        String xmlString = result.getWriter().toString();
        log.debug("The xml string is " + xmlString);
        metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
        metaDataMap.put("submittedDataString", xmlString);
      }

    } catch (Exception e) {
      
      log.debug(e.getMessage());
    }

  }

}
```
