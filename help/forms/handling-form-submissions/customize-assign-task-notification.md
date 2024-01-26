---
title: Personnaliser la notification d’affectation de tâche
description: Inclure les données de formulaire dans les e-mails de notification de tâche
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-6279
thumbnail: KT-6279.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 0cb74afd-87ff-4e79-a4f4-a4634ac48c51
last-substantial-update: 2020-07-07T00:00:00Z
duration: 151
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 100%

---

# Personnaliser la notification d’affectation de tâche

Le composant Attribuer une tâche permet d’attribuer des tâches aux personnes participantes du workflow. Lorsqu’une tâche est attribuée à un utilisateur, une utilisatrice ou à un groupe, une notification par e-mail est envoyée à l’utilisateur, à l’utilisatrice ou aux personnes membres de groupe, respectivement.
Cette notification par e-mail contient généralement des données dynamiques liées à la tâche. Ces données dynamiques sont récupérées à l’aide des [propriétés de métadonnées](https://experienceleague.adobe.com/docs/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html?lang=fr#using-system-generated-metadata-in-an-email-notification) générées par le système.
Pour inclure des valeurs des données de formulaire envoyées dans la notification par e-mail, nous devons créer une propriété de métadonnées personnalisée, puis utiliser ces propriétés de métadonnées personnalisées dans le modèle d’e-mail.



## Créer une propriété de métadonnées personnalisée

L’approche recommandée consiste à créer un composant OSGI qui implémente la méthode getUserMetadata de [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--).

Le code suivant crée 4 propriétés de métadonnées (_firstName_, _lastName_, _reason_ et _amountRequested_) et définit les valeurs à partir des données envoyées. Par exemple, la valeur de propriété de métadonnées _firstName_ est définie sur la valeur de l’élément appelé firstName des données envoyées. Le code suivant suppose que les données envoyées du formulaire adaptatif sont au format xml. Les formulaires adaptatifs basés sur un schéma JSON ou un modèle de données de formulaire génèrent des données au format JSON.


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

## Utiliser les propriétés de métadonnées personnalisées dans le modèle d’e-mail de notification de tâche

Dans le modèle d’e-mail, vous pouvez inclure la propriété de métadonnées en utilisant la syntaxe suivante, où amountRequested correspond à la propriété de métadonnées `${amountRequested}`.

## Configurer l’attribution des tâches pour utiliser une propriété de métadonnées personnalisée

Une fois le composant OSGi créé et déployé sur le serveur AEM, configurez le composant d’attribution des tâches comme illustré ci-dessous pour utiliser les propriétés de métadonnées personnalisées.


![Notification de tâche.](assets/task-notification.PNG)

## Activer l’utilisation de propriétés de métadonnées personnalisées

![Propriétés des métadonnées personnalisées.](assets/custom-meta-data-properties.PNG)

## Pour essayer ceci sur votre serveur

* [Configuration du service de messagerie Day CQ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=fr#configuring-the-mail-service)
* Associez un ID d’e-mail valide à une [personne administratrice](http://localhost:4502/security/users.html).
* Téléchargez et installez le [Workflow-and-notification-template](assets/workflow-and-task-notification-template.zip) avec le [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Téléchargez le [formulaire adaptatif](assets/request-travel-authorization.zip) et importez-le dans AEM via l’[interface utilisateur des formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Déployez et démarrez le [lot personnalisé](assets/work-items-user-service-bundle.jar) en utilisant la [console web](http://localhost:4502/system/console/bundles).
* [Prévisualiser et envoyer le formulaire](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Lors de l’envoi du formulaire, une notification d’affectation de tâche est envoyée à l’adresse e-mail associée à la personne administratrice. La capture d’écran suivante présente un exemple de notification d’affectation de tâche.

![Notification.](assets/task-nitification-email.png)

>[!NOTE]
>Le modèle d’e-mail pour la notification d’affectation de tâche doit être au format suivant.
>
> objet = tâche affectée - `${workitem_title}`
>
> message = chaîne représentant votre modèle d’e-mail sans aucun caractère de nouvelle ligne.

## Commentaires de tâche dans l’e-mail de notification d’affectation de tâche

Dans certains cas, vous souhaiterez peut-être inclure les commentaires de la précédente personne propriétaire de la tâche dans les notifications de tâche. Le code permettant de capturer le dernier commentaire de la tâche est répertorié ci-dessous :

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

Le lot avec le code ci-dessus peut être [téléchargé ici](assets/samples.aemforms.taskcomments.taskcomments.core-1.0-SNAPSHOT.jar).
