---
title: Assemblage de pièces jointes de formulaire
description: Assemblage de pièces jointes de formulaire dans l’ordre spécifié
feature: Assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# Assemblage de pièces jointes de formulaire

Cet article fournit des ressources pour assembler des pièces jointes de formulaire adaptatif dans un ordre spécifié. Les pièces jointes du formulaire doivent être au format pdf pour que cet exemple de code fonctionne. Voici le cas d’utilisation.
L’utilisateur qui remplit un formulaire adaptatif joint un ou plusieurs documents pdf au formulaire.
Lors de l’envoi du formulaire, assemblez les pièces jointes du formulaire pour générer un pdf. Vous pouvez spécifier l’ordre dans lequel les pièces jointes sont assemblées pour générer le pdf final.

## Créer un composant OSGi qui implémente l’interface WorkflowProcess

Créez un composant OSGi qui implémente l’interface [com.adobe.granite.workflow.exec.WorkflowProcess](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). Le code de ce composant peut être associé au composant d’étape de processus dans le workflow AEM. La méthode execute de l’interface com.adobe.granite.workflow.exec.WorkflowProcess est implémentée dans ce composant.

Lorsqu’un formulaire adaptatif est envoyé pour déclencher un processus d’AEM, les données envoyées sont stockées dans le fichier spécifié sous le dossier de charge utile. Il s’agit, par exemple, du fichier de données envoyé. Nous devons assembler les pièces jointes spécifiées sous la balise &quot;idcard&quot; et &quot;bankstatement&quot;.
![submit-data](assets/submitted-data.JPG).

### Obtention des noms de balise

L’ordre des pièces jointes est spécifié en tant qu’arguments d’étape du processus dans le workflow, comme illustré dans la capture d’écran ci-dessous. Ici, nous assemblons les pièces jointes ajoutées à la carte d&#39;identité du champ suivies de relevés de compte.

![process-step](assets/process-step.JPG)

Le fragment de code suivant extrait les noms des pièces jointes des arguments de processus.

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Création de DDX à partir des noms de pièces jointes

Nous devons ensuite créer le document [Document Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) utilisé par le service Assembler pour assembler des documents. Voici le DDX créé à partir des arguments de processus. L’élément NoForms vous permet d’aplatir les documents basés sur XFA avant qu’ils ne soient assemblés. Notez que les éléments de la source PDF sont dans le bon ordre, comme indiqué dans les arguments de processus.

![ddx-xml](assets/ddx.PNG)

### Créer une carte des documents

Nous créons ensuite une carte des documents avec le nom de la pièce jointe comme clé et la pièce jointe comme valeur. Query Builder a été utilisé pour interroger les pièces jointes sous le chemin de charge utile et créer la carte des documents. Cette carte du document avec le DDX est nécessaire pour que le service Assembler assemble le pdf final.

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### Utilisation d’AssemblerService pour assembler les documents

Une fois le DDX et le document map créés, l’étape suivante consiste à utiliser AssemblerService pour assembler les documents.
Le code suivant assemble et renvoie le pdf assemblé.

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### Enregistrez le fichier pdf assemblé sous le dossier de charge utile.

La dernière étape consiste à enregistrer le fichier pdf assemblé sous le dossier de charge utile. Ce pdf est ensuite accessible dans les étapes suivantes du workflow pour un traitement ultérieur.
Le fragment de code suivant a été utilisé pour enregistrer le fichier sous le dossier de charge utile.

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

Voici la structure de dossiers de charge utile après l’assemblage et le stockage des pièces jointes du formulaire.

![payload-structure](assets/payload-structure.JPG)

### Pour que cette fonctionnalité fonctionne sur votre serveur AEM

* Téléchargez le formulaire [Assembler les pièces jointes de formulaire](assets/assemble-form-attachments-af.zip) sur votre système local.
* Importez le formulaire à partir de la page [Forms And Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Téléchargez [workflow](assets/assemble-form-attachments.zip) et importez-le dans AEM à l’aide du gestionnaire de modules.
* Téléchargez le [lot personnalisé](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* Déployez et démarrez le lot à l’aide de la [console web](http://localhost:4502/system/console/bundles)
* Pointez votre navigateur sur [Formulaire AssembleAttachments](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Ajoutez une pièce jointe dans le document d’identification et quelques documents pdf à la section des relevés de banque.
* Envoyer le formulaire pour déclencher le workflow
* Vérifiez le dossier de charge utile [du workflow dans le crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) pour le pdf assemblé.

>[!NOTE]
> Si vous avez activé l’enregistreur pour le lot personnalisé, le DDX et le fichier assemblé sont écrits dans le dossier de votre installation AEM.

