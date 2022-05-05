---
title: Personnalisation de la notification d’affectation de tâche
description: Inclure les données de formulaire dans les e-mails de notification de tâche
sub-product: forms
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
source-git-commit: eb2a807587ab918be82d00d50bf1b338df58e84c
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 6%

---

# Personnalisation de la notification d’affectation de tâche

Le composant Affecter une tâche permet d’affecter des tâches aux participants du workflow. Lorsqu’une tâche est affectée à un utilisateur ou à un groupe, une notification électronique est envoyée à l’utilisateur ou aux membres de groupe définis.
Cette notification par e-mail contient généralement des données dynamiques liées à la tâche. Ces données dynamiques sont récupérées à l’aide du système généré. [propriétés de métadonnées](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification).
Pour inclure des valeurs des données de formulaire envoyées dans la notification par e-mail, nous devons créer une propriété de métadonnées personnalisée, puis utiliser ces propriétés de métadonnées personnalisées dans le modèle de courrier électronique.



## Création d’une propriété de métadonnées personnalisée

L’approche recommandée consiste à créer un composant OSGI qui implémente la méthode getUserMetadata de [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--)

Le code suivant crée 4 propriétés de métadonnées (_firstName_,_lastName_,_reason_ et _amountRequested_) et définit sa valeur à partir des données envoyées. Par exemple, la propriété de métadonnées _firstName_ La valeur de est définie sur la valeur de l’élément appelé firstName des données envoyées. Le code suivant suppose que les données envoyées du formulaire adaptatif sont au format xml. Les Forms adaptatives basées sur un schéma JSON ou un modèle de données de formulaire génèrent des données au format JSON.


```java
package com.aemforms.workitemuserservice.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.*;


import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;
@Component(property={Constants.SERVICE_DESCRIPTION+"=A sample implementation of a user metadata service.",
Constants.SERVICE_VENDOR+"=Adobe Systems",
"process.label"+"=Sample Custom Metadata Service"})


public class WorkItemUserServiceImpl implements WorkitemUserMetadataService {
private static final Logger log = LoggerFactory.getLogger(WorkItemUserServiceImpl.class);

@Override
public Map<String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession,MetaDataMap metadataMap)
{
HashMap<String, String> customMetadataMap = new HashMap<String, String>();
String payloadPath = workItem.getWorkflowData().getPayload().toString();
String dataFilePath = payloadPath + "/Data.xml/jcr:content";
Session session = workflowSession.adaptTo(Session.class);
DocumentBuilderFactory factory = null;
DocumentBuilder builder = null;
Document xmlDocument = null;
javax.jcr.Node xmlDataNode = null;
try
{
    xmlDataNode = session.getNode(dataFilePath);
    InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
    XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
    factory = DocumentBuilderFactory.newInstance();
    builder = factory.newDocumentBuilder();
    xmlDocument = builder.parse(xmlDataStream);
    Node firstNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/firstName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    log.debug("The value of first name element  is " + firstNameNode.getTextContent());
    Node lastNameNode = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/lastName")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node amountRequested = (org.w3c.dom.Node) xPath
            .compile("afData/afUnboundData/data/amountRequested")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    Node reason = (org.w3c.dom.Node) xPath.compile("afData/afUnboundData/data/reason")
            .evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
    customMetadataMap.put("firstName", firstNameNode.getTextContent());
    customMetadataMap.put("lastName", lastNameNode.getTextContent());
    customMetadataMap.put("amountRequested", amountRequested.getTextContent());
    customMetadataMap.put("reason", reason.getTextContent());
    log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

}
catch (Exception e)
{
    log.debug(e.getMessage());
}
return customMetadataMap;
}

}
```

## Utiliser les propriétés de métadonnées personnalisées dans le modèle d’email de notification de tâche

Dans le modèle de courrier électronique, vous pouvez inclure la propriété de métadonnées en utilisant la syntaxe suivante, où amountRequested correspond à la propriété de métadonnées. `${amountRequested}`

## Configuration d’Assign Task pour utiliser une propriété de métadonnées personnalisée

Une fois le composant OSGi créé et déployé sur AEM serveur, configurez le composant Assign Task comme illustré ci-dessous pour utiliser les propriétés de métadonnées personnalisées.


![Notification de tâche](assets/task-notification.PNG)

## Activation de l’utilisation de propriétés de métadonnées personnalisées

![Propriétés Custom Meta Data](assets/custom-meta-data-properties.PNG)

## Pour essayer cela sur votre serveur

* [Configuration du service de messagerie Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=fr#configuring-the-mail-service)
* Associer un ID de courrier électronique valide à [utilisateur administrateur](http://localhost:4502/security/users.html)
* Téléchargez et installez le [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) using [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Télécharger [Formulaire adaptatif](assets/request-travel-authorization.zip) et importez dans AEM à partir du [Interface utilisateur des formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Déployez et démarrez le [Bundle personnalisé](assets/work-items-user-service-bundle.jar) en utilisant la variable [console web](http://localhost:4502/system/console/bundles)
* [Aperçu et envoi du formulaire](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Une fois la tâche d’affectation envoyée au formulaire, la notification est envoyée à l’ID de courrier électronique associé à l’utilisateur administrateur. La capture d’écran suivante présente un exemple de notification d’affectation de tâche

![Notification](assets/task-nitification-email.png)

>[!NOTE]
>Le modèle de courrier électronique pour la notification de tâche d’affectation doit être au format suivant.
>
> subject=Task Assigned - `${workitem_title}`
>
> message=Chaîne représentant votre modèle d’email sans aucun nouveau caractère de ligne.

## Notification par e-mail Task Comments dans Assign Task

Dans certains cas, vous souhaiterez peut-être inclure les commentaires du propriétaire précédent de la tâche dans les notifications de tâche suivantes. Le code permettant de capturer le dernier commentaire de la tâche est répertorié ci-dessous :

```java
package samples.aemforms.taskcomments.core;

import org.osgi.service.component.annotations.Component;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.HistoryItem;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.metadata.MetaDataMap;

import com.adobe.fd.workspace.service.external.WorkitemUserMetadataService;
@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=A sample implementation of a user metadata service.",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Capture Workflow Comments"
})

public class CaptureTaskComments implements WorkitemUserMetadataService {
  private static final Logger log = LoggerFactory.getLogger(CaptureTaskComments.class);
  @Override
  public Map <String, String> getUserMetadata(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap metadataMap) {
    HashMap < String, String > customMetadataMap = new HashMap < String, String > ();
    workflowSession.adaptTo(Session.class);
    try {
      List <HistoryItem> workItemsHistory = workflowSession.getHistory(workItem.getWorkflow());
      int listSize = workItemsHistory.size();
      HistoryItem lastItem = workItemsHistory.get(listSize - 1);
      String reviewerComments = (String) lastItem.getWorkItem().getMetaDataMap().get("workitemComment");
      log.debug("####The comment I got was ...." + reviewerComments);
      customMetadataMap.put("comments", reviewerComments);
      log.debug("Created  " + customMetadataMap.size() + " metadata  properties");

    } catch (Exception e) {
      log.debug(e.getMessage());
    }
    return customMetadataMap;
  }

}
```

Le lot avec le code ci-dessus peut être [téléchargé ici](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar)
