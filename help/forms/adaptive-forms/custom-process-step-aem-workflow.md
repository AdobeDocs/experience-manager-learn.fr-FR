---
title: Mise en oeuvre d’une étape de processus personnalisée
seo-title: Mise en oeuvre d’une étape de processus personnalisée
description: Écriture de pièces jointes de formulaire adaptatif dans un système de fichiers à l’aide d’une étape de processus personnalisée
seo-description: Écriture de pièces jointes de formulaire adaptatif dans un système de fichiers à l’aide d’une étape de processus personnalisée
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 4%

---


# Étape de processus personnalisée

Ce tutoriel est destiné aux clients AEM Forms qui doivent mettre en oeuvre une étape de processus personnalisée. Une étape de processus peut exécuter un script ECMA ou appeler du code Java personnalisé pour effectuer des opérations. Ce tutoriel explique les étapes nécessaires à la mise en oeuvre de WorkflowProcess qui est exécuté par l’étape de processus.

La principale raison de la mise en oeuvre de l’étape de processus personnalisée est d’étendre le workflow AEM. Par exemple, si vous utilisez des composants AEM Forms dans votre modèle de workflow, vous pouvez effectuer les opérations suivantes :

* Enregistrer la ou les pièces jointes du formulaire adaptatif dans le système de fichiers
* Manipuler les données envoyées

Pour réaliser le cas d’utilisation ci-dessus, vous écrirez généralement un service OSGi qui est exécuté par l’étape de processus.

## Créer un projet Maven

La première étape consiste à créer un projet Maven à l’aide de l’archétype Maven d’Adobe approprié. Les étapes détaillées sont répertoriées dans cet [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en). Une fois votre projet Maven importé dans eclipse, vous êtes prêt à commencer à écrire votre premier composant OSGi qui peut être utilisé dans votre étape de processus.


### Créer une classe qui implémente WorkflowProcess

Ouvrez le projet Maven dans votre IDE eclipse. Développez le dossier **nom_projet** > **core**. Développez le dossier src/main/java . Vous devriez voir un package qui se termine par &quot;core&quot;. Créez une classe Java qui implémente WorkflowProcess dans ce module. Vous devez remplacer la méthode d’exécution. La signature de la méthode execute est la suivante :
public void execute(WorkItem workItem, WorkflowSessionSession, MetaDataMap processArguments)renvoie WorkflowException
La méthode execute donne accès aux 3 variables suivantes :

**WorkItem** : La variable workItem donne accès aux données liées au workflow. La documentation de l’API publique est disponible [ici.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession** : Cette variable workflowSession vous permet de contrôler le workflow. La documentation de l’API publique est disponible [ici](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap** : Toutes les métadonnées associées au workflow. Tous les arguments de processus transmis à l’étape de processus sont disponibles à l’aide de l’objet MetaDataMap .[Documentation d’API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

Dans ce tutoriel, nous allons écrire les pièces jointes ajoutées au formulaire adaptatif dans le système de fichiers dans le cadre du processus AEM.

Pour réaliser ce cas d’utilisation, la classe Java suivante a été écrite :

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

Ligne 1 - définit les propriétés de notre composant. La propriété process.label est ce que vous verrez lors de l’association du composant OSGi à l’étape du processus, comme illustré dans l’une des captures d’écran ci-dessous.

Lignes 13 à 15 - Les arguments de processus transmis à ce composant OSGi sont fractionnés à l’aide du séparateur &quot;,&quot;. Les valeurs de attachmentPath et saveToLocation sont ensuite extraites du tableau de chaînes.

* attachmentPath : il s’agit du même emplacement que celui que vous avez spécifié dans le formulaire adaptatif lorsque vous avez configuré l’action d’envoi du formulaire adaptatif pour appeler AEM processus. Il s’agit du nom du dossier dans lequel vous souhaitez que les pièces jointes soient enregistrées dans AEM par rapport à la charge utile du workflow.

* saveToLocation : emplacement où vous souhaitez que les pièces jointes soient enregistrées sur le système de fichiers de votre serveur AEM.

Ces deux valeurs sont transmises en tant qu’arguments de processus, comme illustré dans la capture d’écran ci-dessous.

![ProcessStep](assets/implement-process-step.gif)

Le service QueryBuilder est utilisé pour interroger des noeuds de type nt:file sous le dossier attachmentsPath . Le reste du code parcourt les résultats de recherche pour créer l’objet Document et l’enregistrer dans le système de fichiers.


>[!NOTE]
>
>Puisque nous utilisons l’objet Document spécifique à AEM Forms, vous devez inclure la dépendance aemfd-client-sdk dans votre projet Maven. L’ID de groupe est com.adobe.aemfd et l’ID d’artefact est aemfd-client-sdk.

#### Création et déploiement

[Créez le lot comme décrit ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/create-your-first-osgi-bundle.html?lang=en#build-your-project)
[ici : assurez-vous que le lot est déployé et en principal état.](http://localhost:4502/system/console/bundles)

Créer un modèle de processus. Effectuez un glisser-déposer de l’étape de processus dans le modèle de workflow. Associez l’étape de processus à &quot;Enregistrer les pièces jointes de formulaire adaptatif dans le système de fichiers&quot;.

Fournissez les arguments de processus nécessaires séparés par une virgule. Par exemple, Pièces jointes, c:\\scrappp\\. Le premier argument est le dossier dans lequel vos pièces jointes de formulaire adaptatif seront stockées par rapport à la charge utile du workflow. Il doit s’agir de la même valeur que celle spécifiée lors de la configuration de l’action d’envoi du formulaire adaptatif. Le deuxième argument correspond à l’emplacement où vous souhaitez que les pièces jointes soient stockées.

Création d’un formulaire adaptatif. Faites glisser et déposez le composant Pièces jointes sur le formulaire. Configurez l’action d’envoi du formulaire pour appeler le workflow créé lors des étapes précédentes. Indiquez le chemin d’accès à la pièce jointe approprié.

Enregistrez les paramètres.

Prévisualiser le formulaire. Ajoutez quelques pièces jointes et envoyez le formulaire. Les pièces jointes doivent être enregistrées dans le système de fichiers à l’emplacement spécifié par vous dans le workflow.

