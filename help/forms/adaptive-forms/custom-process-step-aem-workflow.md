---
title: Mettre en œuvre une étape de processus personnalisée
description: Écrire des pièces jointes de formulaire adaptatif dans un système de fichiers à l’aide d’une étape de processus personnalisée
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
last-substantial-update: 2021-06-09T00:00:00Z
duration: 234
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '769'
ht-degree: 100%

---

# Étape de processus personnalisée

Ce tutoriel est destiné aux clientes et clients AEM Forms qui doivent mettre en œuvre une étape de processus personnalisée. Une étape de processus peut exécuter un script ECMA ou appeler du code Java personnalisé pour effectuer des opérations. Ce tutoriel explique les étapes nécessaires à la mise en œuvre de WorkflowProcess, exécuté par l’étape de processus.

La principale raison de la mise en œuvre de l’étape de processus personnalisée est d’étendre le workflow AEM. Par exemple, si vous utilisez des composants AEM Forms dans votre modèle de workflow, vous pouvez effectuer les opérations suivantes :

* Enregistrer la ou les pièces jointes du formulaire adaptatif dans le système de fichiers
* Traiter les données envoyées

Pour réaliser le cas d’utilisation ci-dessus, il faut généralement écrire un service OSGi qui est ensuite exécuté par l’étape de processus.

## Créer le projet Maven

La première étape consiste à créer un projet Maven à l’aide de l’archétype Maven Adobe approprié. Les étapes détaillées sont répertoriées dans cet [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=fr). Une fois votre projet Maven importé dans Eclipse, vous êtes en mesure de commencer à écrire votre premier composant OSGi qui peut être utilisé dans votre étape de processus.


### Créer une classe qui implémente WorkflowProcess

Ouvrez le projet Maven dans votre IDE Eclipse. Développez le dossier **projectname** > **core**. Développez le dossier src/main/java. Vous devriez voir un package qui se termine par « core ». Créez une classe Java qui implémente WorkflowProcess dans ce package. Vous devez remplacer la méthode d’exécution. La signature de la méthode d’exécution est la suivante :
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)throws WorkflowException
La méthode d’exécution donne accès aux 3 variables suivantes :

**WorkItem** : la variable workItem donne accès aux données relatives au workflow. La documentation de l’API publique est disponible [ici.](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession** : cette variable workflowSession vous donne la possibilité de contrôler le workflow. La documentation de l’API publique est disponible [ici.](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap** : toutes les métadonnées associées au workflow. Tous les arguments de processus transmis à l’étape de processus sont disponibles à l’aide de l’objet MetaDataMap.[Documentation de l’API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

Dans ce tutoriel, nous allons écrire les pièces jointes ajoutées au formulaire adaptatif dans le système de fichiers dans le cadre du workflow AEM.

Pour réaliser ce cas d’utilisation, la classe Java suivante a été écrite :

Regardons ce code.

```java
package com.learningaemforms.adobe.core;

import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;

@Component(property = {
    Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
    Constants.SERVICE_VENDOR + "=Adobe Systems",
    "process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
    @Reference
    QueryBuilder queryBuilder;

    @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
    throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        Map<String, String> map = new HashMap<String, String> ();
        map.put("path", payloadPath + "/" + attachmentsPath);
        File saveLocationFolder = new File(saveToLocation);
        if (!saveLocationFolder.exists()) {
            saveLocationFolder.mkdirs();
        }

        map.put("type", "nt:file");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
        query.setStart(0);
        query.setHitsPerPage(20);

        SearchResult result = query.getResult();
        log.debug("Got  " + result.getHits().size() + " attachments ");
        Node attachmentNode = null;
        for (Hit hit: result.getHits()) {
            try {
                String path = hit.getPath();
                log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
                attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
                InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
                Document attachmentDoc = new Document(documentStream);
                attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
                attachmentDoc.close();
            } catch (Exception e) {
                log.debug("Error saving file " + e.getMessage());
            }
```

Ligne 1 : définit les propriétés de notre composant. La propriété process.label est celle que vous verrez lors de l’association du composant OSGi à l’étape de processus, comme illustré dans l’une des captures d’écran ci-dessous.

Lignes 13 à 15 : les arguments de processus transmis à ce composant OSGi sont fractionnés à l’aide du séparateur « , ». Les valeurs d’attachmentPath et de saveToLocation sont ensuite extraites du tableau de chaînes.

* attachmentPath : il s’agit de l’emplacement que vous avez spécifié dans le formulaire adaptatif lorsque vous avez configuré l’action d’envoi du formulaire adaptatif pour invoquer un workflow AEM. Il s’agit du nom du dossier dans lequel vous souhaitez que les pièces jointes soient enregistrées dans AEM par rapport à la payload du workflow.

* saveToLocation : il s’agit de l’emplacement où vous souhaitez que les pièces jointes soient enregistrées sur le système de fichiers de votre serveur AEM.

Ces deux valeurs sont transmises en tant qu’arguments de processus, comme illustré dans la capture d’écran ci-dessous.

![ProcessStep](assets/implement-process-step.gif)

Le service QueryBuilder est utilisé pour interroger des nœuds de type nt:file sous le dossier attachmentsPath. Le reste du code parcourt les résultats de recherche pour créer l’objet Document et l’enregistrer dans le système de fichiers.


>[!NOTE]
>
>Puisque nous utilisons l’objet Document spécifique à AEM Forms, vous devez inclure la dépendance aemfd-client-sdk dans votre projet Maven. L’ID de groupe est com.adobe.aemfd et l’ID d’artefact est aemfd-client-sdk.

#### Créer et déployer

[Créez le lot comme décrit ici.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html?lang=fr)
[Assurez-vous que le bundle est déployé et en état actif.](http://localhost:4502/system/console/bundles)

Créez un modèle de workflow. Effectuez un glisser-déposer de l’étape de processus dans le modèle de workflow. Associez l’étape de processus à « Enregistrer les pièces jointes de formulaire adaptatif dans le système de fichiers ».

Fournissez les arguments de processus nécessaires séparés par une virgule. Par exemple, Attachments,c:\\scrappp\\. Le premier argument est le dossier dans lequel les pièces jointes de votre formulaire adaptatif seront stockées par rapport à la payload du workflow. Il doit s’agir de la même valeur que celle spécifiée lors de la configuration de l’action d’envoi du formulaire adaptatif. Le deuxième argument correspond à l’emplacement où vous souhaitez stocker les pièces jointes.

Créez un formulaire adaptatif. Faites glisser et déposez le composant Attachments (pièces jointes) du fichier sur le formulaire. Configurez l’action d’envoi du formulaire pour appeler le workflow créé lors des étapes précédentes. Indiquez le chemin d’accès à la pièce jointe approprié.

Enregistrez les paramètres.

Prévisualisez le formulaire. Ajoutez des pièces jointes et envoyez le formulaire. Les pièces jointes doivent être enregistrées dans le système de fichiers à l’emplacement que vous avez spécifié dans le workflow.
