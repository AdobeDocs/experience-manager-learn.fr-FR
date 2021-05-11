---
title: Créer une étape de processus personnalisée
description: Étape de processus personnalisée pour convertir un mot, excel les pièces jointes au format PDF à l’aide de Document Cloud.
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: développement
thumbnail: 331891.jpg
kt: 7192
translation-type: tm+mt
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 1%

---

# Étape de processus personnalisée

Voici le code complet de l’étape de processus personnalisé qui convertit et remplace les fichiers natifs par les fichiers pdf convertis.
Cette étape personnalisée recherche toutes les pièces jointes sous le nom de dossier fourni en tant qu’argument de processus dans le processus.
Cette étape de processus personnalisé utilise les méthodes de DocumentCloudSDKService personnalisé pour créer des fichiers PDF.


```java
package com.aemforms.doccloud.core;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Binary;
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
import com.day.cq.search.result.SearchResult;;

@Component(property = {
	Constants.SERVICE_DESCRIPTION + "=Convert form attachments to PDF Using Document Cloud API",
	Constants.SERVICE_VENDOR + "=Adobe Systems",
	"process.label" + "=Convert form attachments to PDF Using Document Cloud API"
})

public class ConvertAttachmentsToPDF implements WorkflowProcess {
	private static final Logger log = LoggerFactory.getLogger(ConvertAttachmentsToPDF.class);
	@Reference
	QueryBuilder queryBuilder;
	@Reference
	DocumentCloudSDKService documentCloudService;
	@Override
	public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments) throws WorkflowException {

		log.debug("The payload path is " + workItem.getWorkflowData().getPayload().toString());
		log.debug("The form attachments are stored under  ..." + processArguments.get("PROCESS_ARGS", "string").toString() + " folder in the payload");

		Session session = workflowSession.adaptTo(Session.class);
		Map<String, String> map = new HashMap<String, String> ();
		map.put("path", workItem.getWorkflowData().getPayload().toString() + "/" + processArguments.get("PROCESS_ARGS", "string").toString());
		map.put("type", "nt:file");
		Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
		query.setStart(0);
		query.setHitsPerPage(20);

		SearchResult result = query.getResult();
		log.debug("Get result hits " + result.getHits().size());
		Node attachmentNode = null;
		for (Hit hit: result.getHits()) {
			try {
				String path = hit.getPath();
				log.debug("The title of the attachment is  " + hit.getTitle() + " and path " + path);
				String title = hit.getTitle();
				title = title.substring(0, title.indexOf("."));
				attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
				InputStream xmlDataStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
				Document convertedPDF = documentCloudService.createPDFFromInputStream(xmlDataStream, hit.getTitle());
				Node nonPDFNode = session.getNode(path);
				session.move(nonPDFNode.getPath(), nonPDFNode.getParent().getPath() + "/" + title + ".pdf");
				Binary binary = session.getValueFactory().createBinary(convertedPDF.getInputStream());
				attachmentNode.setProperty("jcr:data", binary);
				log.debug("Replacing the original file with converted pdf ");
				session.save();

			} catch (Exception e) {
				System.out.println(e.getMessage());
			}
		}

	}

}
```
