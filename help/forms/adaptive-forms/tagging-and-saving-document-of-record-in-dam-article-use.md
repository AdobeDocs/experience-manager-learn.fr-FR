---
title: Balisage et stockage d’un document d’enregistrement AEM Forms dans la gestion des ressources numériques
description: Cet article décrit le cas pratique de stockage et de balisage du document d’enregistrement généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 1%

---

# Balisage et stockage d’un document d’enregistrement AEM Forms dans la gestion des ressources numériques {#tagging-and-storing-aem-forms-dor-in-dam}

Cet article décrit le cas pratique de stockage et de balisage du document d’enregistrement généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.

Les clients ont généralement pour tâche de stocker et de baliser le document d’enregistrement généré par AEM Forms dans AEM DAM. Le balisage du document doit être basé sur les données envoyées par la Forms adaptative. Par exemple, si l’état de l’emploi dans les données envoyées est &quot;Retiré&quot;, nous voulons baliser le document avec la balise &quot;Retiré&quot; et le stocker dans la gestion des ressources numériques.

Le cas pratique est le suivant :

* Un utilisateur remplit un formulaire adaptatif. Dans le formulaire adaptatif, l’état civil de l’utilisateur (ex Célibataire) et l’état de l’emploi (ex. Retraité) sont capturés.
* Lors de l’envoi du formulaire, un workflow d’AEM est déclenché. Ce processus balise le document avec l’état civil (célibataire) et l’état de travail (retraité) et stocke le document dans la gestion des ressources numériques.
* Une fois le document stocké dans DAM, l’administrateur doit pouvoir le rechercher à l’aide de ces balises. Par exemple, la recherche sur un document d’enregistrement unique ou à la retraite récupérerait les DE appropriés.

Pour répondre à ce cas d’utilisation, une étape de processus personnalisée a été écrite. Au cours de cette étape, nous récupérons les valeurs des éléments de données appropriés à partir des données envoyées. Nous construisons ensuite la mosaïque de balise à l’aide de cette valeur. Par exemple, si la valeur de l’élément d’état civil est &quot;Célibataire&quot;, le titre de la balise devient **Pic:EmploiStatus/Célibataire. **À l’aide de l’ API TagManager , nous recherchons la balise et nous l’appliquons au DE.

Voici le code complet pour baliser et stocker le document d’enregistrement dans AEM DAM.

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

Pour que cet exemple fonctionne sur votre système, procédez comme suit :
* [Déployer le lot Developer withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Télécharger et déployer le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui définit les balises à partir des données de formulaire envoyées.

* [Téléchargement de l’exemple de formulaire adaptatif](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Accès à Forms et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Cliquez sur Créer | Téléchargez le fichier et téléchargez le fichier tag-and-store-in-dam-adaptive-form.zip

* [Importation des actifs de l’article](assets/tag-and-store-in-dam-assets.zip) utilisation d’AEM gestionnaire de packages
* Ouvrez le [exemple de formulaire en mode aperçu](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **Renseignez tous les champs** et envoyez le formulaire.
* [Accédez au dossier Pic dans DAM](http://localhost:4502/assets.html/content/dam/Peak). Vous devriez voir le document d’enregistrement dans le dossier Pic . Vérifiez les propriétés du document. Il doit être correctement balisé.
Félicitations!! Vous avez installé l’exemple sur votre système.

* Explorons le [workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) qui est déclenché lors de l’envoi du formulaire.
* La première étape du workflow crée un nom de fichier unique en concaténant le nom et le lieu de résidence des candidats.
* La deuxième étape du workflow transmet la hiérarchie de balises et les éléments de champs de formulaire qui doivent être balisés. L’étape de processus extrait la valeur des données envoyées et construit le titre de la balise qui doit marquer le document.
* Si vous souhaitez stocker un document d’enregistrement dans un autre dossier de la gestion des actifs numériques, vous devez spécifier l’emplacement du dossier à l’aide des propriétés de configuration, comme indiqué dans la capture d’écran ci-dessous.

Les deux autres paramètres sont spécifiques au DE et au chemin d’accès au fichier de données, comme indiqué dans les options d’envoi du formulaire adaptatif. Assurez-vous que les valeurs que vous spécifiez ici correspondent aux valeurs que vous avez spécifiées dans les options d’envoi du formulaire adaptatif.

![Balise Dor](assets/tag_dor_service_configuration.gif)
