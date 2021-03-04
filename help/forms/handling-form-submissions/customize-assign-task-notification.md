---
title: Personnaliser la notification d'affectation de Tâche
description: Inclure les données de formulaire dans les e-mails de notification d'affectation de tâche
sub-product: formulaires
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 6279
thumbnail: KT-6279.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 7%

---


# Personnaliser la notification d&#39;affectation de Tâche

Le composant Affecter une Tâche est utilisé pour affecter des tâches aux participants au processus. Lorsqu’une tâche est affectée à un utilisateur ou à un groupe, une notification par courrier électronique est envoyée à l’utilisateur ou aux membres de groupe définis.
Cette notification par courrier électronique contient généralement des données dynamiques liées à la tâche. Ces données dynamiques sont extraites à l’aide des propriétés de [métadonnées ](https://docs.adobe.com/content/help/en/experience-manager-65/forms/publish-process-aem-forms/use-metadata-in-email-notifications.html#using-system-generated-metadata-in-an-email-notification) générées par le système.
Pour inclure des valeurs issues des données de formulaire envoyées dans la notification par courrier électronique, nous devons créer une propriété de métadonnées personnalisée, puis utiliser ces propriétés de métadonnées personnalisées dans le modèle de courrier électronique.



## Création d’une propriété de métadonnées personnalisée

L’approche recommandée consiste à créer un composant OSGI qui implémente la méthode getUserMetadata de [WorkitemUserMetadataService](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/com/adobe/fd/workspace/service/external/WorkitemUserMetadataService.html#getUserMetadataMap--).

Le code suivant crée 4 propriétés de métadonnées (_firstName_,_lastName_,_reason_ et _amountRequested_) et définit sa valeur à partir des données envoyées. Par exemple, la valeur de la propriété de métadonnées _firstName_ est définie sur la valeur de l’élément nommé firstName à partir des données envoyées. Le code suivant suppose que les données envoyées du formulaire adaptatif sont au format xml. Forms adaptatif basé sur le schéma JSON ou le modèle de données de formulaire génère des données au format JSON.


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

## Utiliser les propriétés de métadonnées personnalisées dans le modèle de courrier électronique de notification de tâche

Dans le modèle de courrier électronique, vous pouvez inclure la propriété metadata en utilisant la syntaxe suivante, où amountRequested correspond à la propriété de métadonnées `${amountRequested}`

## Configuration de l’affectation d’une Tâche pour l’utilisation d’une propriété de métadonnées personnalisée

Une fois le composant OSGi créé et déployé sur AEM serveur, configurez le composant Assign Tâche comme illustré ci-dessous pour utiliser les propriétés de métadonnées personnalisées.


![Notification de tâche](assets/task-notification.PNG)

## Activer l’utilisation des propriétés de métadonnées personnalisées

![Propriétés de métadonnées personnalisées](assets/custom-meta-data-properties.PNG)

## Pour essayer ceci sur votre serveur

* [Configuration du service de messagerie Day CQ](https://docs.adobe.com/content/help/fr-FR/experience-manager-65/administering/operations/notification.html#configuring-the-mail-service)
* Associez un ID de courrier électronique valide à [l’utilisateur admin](http://localhost:4502/security/users.html).
* Téléchargez et installez le [modèle de flux de travaux et de notification](assets/workflow-and-task-notification-template.zip) à l’aide de [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Téléchargez [Formulaire adaptatif](assets/request-travel-authorization.zip) et importez-le dans AEM à partir des [formulaires et documents ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Déployez et début le [lot personnalisé](assets/work-items-user-service-bundle.jar) à l’aide de la [console Web](http://localhost:4502/system/console/bundles).
* [Prévisualisation et envoi du formulaire](http://localhost:4502/content/dam/formsanddocuments/requestfortravelauhtorization/jcr:content?wcmmode=disabled)

Une notification d’affectation de tâche d’envoi de formulaire est envoyée à l’ID de courrier électronique associé à l’utilisateur administrateur. La capture d&#39;écran suivante montre un exemple de notification d&#39;affectation de tâche

![Notification](assets/task-nitification-email.png)

>[!NOTE]
>Le modèle de courrier électronique pour la notification d’affectation de tâche doit être au format suivant.
>
> subject=Tâche assignée - `${workitem_title}`
>
> message=Chaîne représentant votre modèle de courrier électronique sans aucun nouveau caractère de ligne.
