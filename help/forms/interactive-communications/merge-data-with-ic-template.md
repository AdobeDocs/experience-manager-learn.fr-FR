---
title: Génération d’un document de canal d’impression à l’aide de la fusion de données
description: Découvrez comment générer un document de canal d’impression en fusionnant les données contenues dans le flux d’entrée
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 3bfbb4ef-0c51-445a-8d7b-43543a5fa191
last-substantial-update: 2019-07-07T00:00:00Z
duration: 151
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '445'
ht-degree: 100%

---

# Générer des documents de canal d’impression à partir des données envoyées

Les documents de canal d’impression sont généralement générés en récupérant des données d’une source de données backend via le service GET du modèle de données de formulaire. Dans certains cas, vous devrez peut-être générer des documents de canal d’impression avec les données fournies. Par exemple : le client ou la cliente remplit le changement de formulaire de destinataire et vous voulez peut-être générer un document de canal d’impression avec les données du formulaire envoyé. Pour ce cas pratique, suivez les étapes suivantes :

## Créer un service de préremplissage

Le nom de service « ccm-print-test » est utilisé pour accéder à ce service. Une fois ce service de préremplissage défini, vous pouvez accéder à ce service dans votre mise en œuvre d’étape de workflow ou de servlet pour générer le document de canal d’impression.

```java
import java.io.InputStream;
import org.osgi.service.component.annotations.Component;

import com.adobe.forms.common.service.ContentType;
import com.adobe.forms.common.service.DataOptions;
import com.adobe.forms.common.service.DataProvider;
import com.adobe.forms.common.service.FormsException;
import com.adobe.forms.common.service.PrefillData;

@Component(immediate = true, service = {DataProvider.class})
public class ICPrefillService implements DataProvider {

@Override
public String getServiceDescription() {
    // TODO Auto-generated method stub
    return "Prefill Service for IC Print Channel";
}

@Override
public String getServiceName() {
    // TODO Auto-generated method stub
    return "ccm-print-test";
}

@Override
public PrefillData getPrefillData(DataOptions options) throws FormsException {
    // TODO Auto-generated method stub
        PrefillData data = null;
        if (options != null && options.getExtras() != null && options.getExtras().get("data") != null) {
            InputStream is = (InputStream) options.getExtras().get("data");
            data = new PrefillData(is, options.getContentType() != null ? options.getContentType() : ContentType.JSON);
        }
        return data;
    }
}
```

### Créer une implémentation WorkflowProcess

Le fragment de code de l’implémentation WorkflowProcess est affiché ci-dessous. Ce code est exécuté lorsque l’étape de processus dans le workflow AEM est associée à cette implémentation. Cette implémentation nécessite 3 arguments de processus, décrits ci-dessous :

* Nom du chemin du fichier de données spécifié lors de la configuration du formulaire adaptatif
* Nom du modèle de canal d’impression
* Nom du document de canal d’impression généré

Ligne 98 - Comme le formulaire adaptatif est basé sur le modèle de données de formulaire, les données résidant dans le nœud de données du afBoundData sont extraites.
Ligne 128 - Le nom du service d’options de données est défini. Notez le nom du service. Il doit correspondre au nom renvoyé dans la ligne 45 de la liste de code précédente.
Ligne 135 - Le document est généré à l’aide de la méthode de rendu de l’objet PrintChannel


```java
String params = arg2.get("PROCESS_ARGS","string").toString();
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    String dataFile = params.split(",")[0];
    final String icFileName = params.split(",")[1];
    String dataFilePath = payloadPath + "/"+dataFile+"/jcr:content";
    Session session = workflowSession.adaptTo(Session.class);
    Node xmlDataNode = null;
    try {
        xmlDataNode = session.getNode(dataFilePath);
        InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
        JsonParser jsonParser = new JsonParser();
        BufferedReader streamReader = null;
        try {
            streamReader = new BufferedReader(new InputStreamReader(xmlDataStream, "UTF-8"));
        } catch (UnsupportedEncodingException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        StringBuilder responseStrBuilder = new StringBuilder();
        String inputStr;
        try {
            while ((inputStr = streamReader.readLine()) != null)
                responseStrBuilder.append(inputStr);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        String submittedDataXml = responseStrBuilder.toString();
        JsonObject jsonObject = jsonParser.parse(submittedDataXml).getAsJsonObject().get("afData").getAsJsonObject()
                .get("afBoundData").getAsJsonObject().get("data").getAsJsonObject();
        logger.info("Successfully Parsed gson" + jsonObject.toString());
        InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
        //InputStream targetStream = new ByteArrayInputStream(jsonObject.toString().getBytes());
        
        // Node dataNode = session.getNode(formName);
        logger.info("Got resource using resource resolver"
                );
        resHelper.callWith(getResolver.getFormsServiceResolver(), new Callable<Void>() {
            @Override
            public Void call() throws Exception {
                System.out.println("The target stream is "+targetStream.available());
                // TODO Auto-generated method stub
                com.adobe.fd.ccm.channels.print.api.model.PrintChannel printChannel = null;
                String formName = params.split(",")[2];
                logger.info("The form name I got was "+formName);
                printChannel = printChannelService.getPrintChannel(formName);
                logger.info("Did i get print channel?");
                com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions options = new com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
                options.setMergeDataOnServer(true);
                options.setRenderInteractive(false);
                com.adobe.forms.common.service.DataOptions dataOptions = new com.adobe.forms.common.service.DataOptions();
                dataOptions.setServiceName(printChannel.getPrefillService());
                // dataOptions.setExtras(map);
                dataOptions.setContentType(ContentType.JSON);
                logger.info("####Set the content type####");
                dataOptions.setFormResource(getResolver.getFormsServiceResolver().getResource(formName));
                dataOptions.setServiceName("ccm-print-test");
                dataOptions.setExtras(new HashMap<String, Object>());
                dataOptions.getExtras().put("data", targetStream);
                options.setDataOptions(dataOptions);
                logger.info("####Set the data options");
                com.adobe.fd.ccm.channels.print.api.model.PrintDocument printDocument = printChannel
                .render(options);
                logger.info("####Generated the document");
                com.adobe.aemfd.docmanager.Document uploadedDocument = new com.adobe.aemfd.docmanager.Document(
                    printDocument.getInputStream());
                logger.info("Generated the document");
                Binary binary = session.getValueFactory().createBinary(printDocument.getInputStream());
                Session jcrSession = workflowSession.adaptTo(Session.class);
                String dataFilePath = workItem.getWorkflowData().getPayload().toString();
                
                Node dataFileNode = jcrSession.getNode(dataFilePath);
                Node icPdf = dataFileNode.addNode(icFileName, "nt:file");
                Node contentNode = icPdf.addNode("jcr:content", "nt:resource");
                contentNode.setProperty("jcr:data", binary);
                jcrSession.save();
                logger.info("Copied the generated document");
                uploadedDocument.close();
                
                return null;
            }
```

Pour réaliser un test sur votre serveur, procédez comme suit :

* [Configuration du service de messagerie Day CQ.](https://experienceleague.adobe.com/docs/experience-manager-65/communities/administer/email.html?lang=fr)Cette opération est nécessaire pour envoyer un e-mail avec le document généré en tant que pièce jointe.
* [Déployez le lot Developing with Service User.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Assurez-vous d’avoir ajouté l’entrée suivante dans la configuration du service de mappeur d’utilisateurs du service Apache Sling.
* **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [Téléchargez et décompressez les ressources liées à cet article sur votre système de fichiers.](assets/prefillservice.zip)
* [Importez les packages suivants à l’aide du gestionnaire de modules AEM](http://localhost:4502/crx/packmgr/index.jsp).
   1. beneficiaryconfirmationic.zip
   2. changeofbeneficiaryform.zip
   3. generatebeneficiaryworkflow.zip
* [Déployez les éléments suivants à l’aide de la console web Felix AEM.](http://localhost:4502/system/console/bundles)

   * GenerateIC.GenerateIC.core-1.0-SNAPSHOT.jar. Ce bundle contient le code mentionné dans cet article.

* [Ouvrez ChangeOfBeneficiaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)
* Assurez-vous que le formulaire adaptatif est configuré pour être envoyé au workflow AEM comme illustré ci-dessous.
  ![image](assets/generateic.PNG)
* [Configurez le modèle de workflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ChangesToBeneficiary.html)Assurez-vous que l’étape de processus et les composants d’envoi d’e-mail sont configurés en fonction de votre environnement.
* [Prévisualisez ChangeOfBeneficiaryForm.](http://localhost:4502/content/dam/formsanddocuments/changebeneficiary/jcr:content?wcmmode=disabled)Renseignez certains détails et effectuez l’envoi.
* Le workflow doit être appelé et le document du canal d’impression IC doit être envoyé au destinataire spécifié ou à la destinataire spécifiée dans le composant d’envoi d’e-mail en tant que pièce jointe.
