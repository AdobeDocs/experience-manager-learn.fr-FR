---
title: Générer un PDF avec les données d’un formulaire adaptatif basé sur un composant principal
description: Fusionner les données de l’envoi de formulaire basé sur les composants principaux avec le modèle XDP dans le workflow
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
source-git-commit: acfd982ce471c8510cb59ed4d353f1d47dcfb5dc
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 2%

---

# Générer un PDF avec les données de l’envoi de formulaire basé sur les composants principaux

Voici le texte révisé en majuscules avec &quot;Core Components&quot; :

Un scénario type implique la génération d’un PDF à partir des données envoyées via un formulaire adaptatif basé sur les composants principaux. Ces données sont toujours au format JSON. Pour générer un PDF à l’aide de l’API Render PDF, il est nécessaire de convertir les données JSON au format XML. La variable `toString` méthode de `org.json.XML` est utilisée pour cette conversion. Pour plus d’informations, reportez-vous au [documentation d’ `org.json.XML.toString` method](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## Formulaire adaptatif basé sur un schéma JSON

Pour créer un schéma JSON pour votre formulaire adaptatif, procédez comme suit :

### Générer des données d’exemple pour le XDP

Pour rationaliser le processus, procédez comme suit :

1. Ouvrez le fichier XDP dans AEM Forms Designer.
1. Accédez à &quot;Fichier&quot; > &quot;Propriétés du formulaire&quot; > &quot;Aperçu&quot;.
1. Sélectionnez &quot;Générer les données d’aperçu&quot;.
1. Cliquez sur &quot;Générer&quot;.
1. Attribuez un nom de fichier significatif, tel que `form-data.xml`.

### Générer un schéma JSON à partir des données XML

Vous pouvez utiliser n’importe quel outil en ligne gratuit pour [Conversion de XML en JSON](https://jsonformatter.org/xml-to-jsonschema) en utilisant les données XML générées à l&#39;étape précédente.

### Processus de workflow personnalisé pour la conversion de JSON en XML

Le code fourni convertit JSON en XML, stockant le code XML obtenu dans une variable de processus de workflow nommée `dataXml`.

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### Créer un workflow

Pour gérer les envois de formulaire, créez un workflow qui comprend deux étapes :

1. L’étape initiale utilise un processus personnalisé pour transformer les données JSON envoyées en données XML.
1. L’étape suivante génère un PDF en combinant les données XML avec le modèle XDP.

![json-to-xml](assets/json-to-xml-process-step.png)


## Déploiement de l’exemple de code

Pour le tester sur votre serveur local, procédez comme suit :

1. [Téléchargez et installez le lot personnalisé via la console web OSGi AEM](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [Importer le module de workflow](assets/workflow_to_render_pdf.zip).
1. [Importez l’exemple de formulaire adaptatif et de modèle XDP.](assets/adaptive_form_and_xdp_template.zip).
1. [Aperçu du formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. Renseignez quelques champs de formulaire.
1. Envoyez le formulaire pour lancer le processus d’AEM.
1. Recherchez le PDF rendu dans le dossier de charge utile du workflow.

