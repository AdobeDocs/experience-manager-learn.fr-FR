---
title: Baliser et stocker un document d’enregistrement AEM Forms dans la gestion des ressources numériques
description: Cet article décrit le cas d’utilisation de stockage et de balisage du document d’enregistrement généré par AEM Forms dans la gestion des ressources numériques AEM. Le balisage du document est effectué en fonction des données de formulaire envoyées.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
last-substantial-update: 2019-03-20T00:00:00Z
duration: 195
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 100%

---

# Baliser et stocker un document d’enregistrement AEM Forms dans la gestion des ressources numériques {#tagging-and-storing-aem-forms-dor-in-dam}

Cet article décrit le cas d’utilisation de stockage et de balisage du document d’enregistrement généré par AEM Forms dans la gestion des ressources numériques AEM. Le balisage du document est effectué en fonction des données de formulaire envoyées.

Une demande courante de la clientèle est le stockage et balisage du document d’enregistrement généré par AEM Forms dans la gestion des ressources numériques AEM. Le balisage du document doit reposer sur les données envoyées par le formulaire adaptatif. Par exemple, si le statut professionnel indiqué dans les données envoyées est « Retraité(e) », ajoutez la balise « Retraité(e) » au document et stockez ce dernier dans la gestion des ressources numériques.

Consultez le cas d’utilisation suivant :

* Un utilisateur ou une utilisatrice remplit un formulaire adaptatif. Dans le formulaire adaptatif, l’état civil de la personne (par exemple, célibataire) et le statut professionnel (par exemple, retraité(e)) sont renseignés.
* L’envoi du formulaire déclenche un workflow AEM. Ce workflow permet d’ajouter les balises relatives à l’état civil (célibataire) et au statut professionnel (retraité(e)) au document et stocke ce dernier dans la gestion des ressources numériques.
* Une fois le document stocké dans la gestion des ressources numériques, l’administrateur ou l’administratrice peut utiliser ces balises pour le rechercher. Par exemple, une recherche portant sur le terme « célibataire » ou « retraité(e) » permet d’identifier le document d’enregistrement pertinent.

Pour répondre à ce cas d’utilisation, une étape de processus personnalisée a été ajoutée. Au cours de cette étape, les valeurs des éléments de données appropriés sont récupérées à partir des données envoyées. Le titre des balises s’inspire de cette valeur. Par exemple, si la valeur de l’élément État civil est « Célibataire », le titre de la balise est le suivant : **Peak:EmploymentStatus/Célibataire. **À l’aide de l’API TagManager, la balise est recherchée et appliquée au document d’enregistrement.

Voici le code complet pour baliser et stocker le document d’enregistrement dans la gestion des ressources numériques AEM.

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

Pour que cet exemple fonctionne sur votre système, procédez comme suit :
* [Déployez le lot Developingwithserviceuser.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et déployez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui définit les balises à partir des données de formulaire envoyées.

* [Téléchargez l’exemple de formulaire adaptatif.](assets/tag-and-store-in-dam-adaptive-form.zip)

* [Accédez à Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).

* Cliquez sur Créer | Charger un fichier et chargez le fichier tag-and-store-in-dam-adaptive-form.zip.

* [Importez les ressources de l’article](assets/tag-and-store-in-dam-assets.zip) à l’aide du gestionnaire de packages AEM.
* Ouvrez l’[exemple de formulaire en mode de prévisualisation](http://localhost:4502/content/dam/formsanddocuments/tagandstoreindam/jcr:content?wcmmode=disabled). **Renseignez tous les champs** et soumettez le formulaire.
* [Accédez au dossier Peak dans la gestion des ressources numériques](http://localhost:4502/assets.html/content/dam/Peak). Le document d’enregistrement doit être présent dans le dossier Peak. Vérifiez les propriétés du document. Il doit disposer des balises appropriées.
Félicitations. Vous avez installé l’exemple sur votre système.

* Penchons-nous sur le [workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) qui est déclenché lors de la soumission du formulaire.
* La première étape du workflow consiste à créer un nom de fichier unique en concaténant le nom et le lieu de résidence des candidates et des candidats.
* La deuxième étape du workflow transmet la hiérarchie de balises et les éléments de champs de formulaire qui doivent être balisés. L’étape de processus extrait la valeur des données envoyées et renseigne le titre de la balise qui doit marquer le document.
* Si vous souhaitez stocker un document d’enregistrement dans un autre dossier de la gestion des ressources numériques, vous devez spécifier l’emplacement du dossier à l’aide des propriétés de configuration, comme indiqué dans la copie d’écran ci-dessous.

Les deux autres paramètres sont spécifiques au document d’enregistrement et au chemin d’accès au fichier de données, comme indiqué dans les options d’envoi du formulaire adaptatif. Assurez-vous que les valeurs que vous renseignez ici correspondent aux valeurs indiquées dans les options d’envoi du formulaire adaptatif.

![Balisage du document d’enregistrement](assets/tag_dor_service_configuration.gif).
