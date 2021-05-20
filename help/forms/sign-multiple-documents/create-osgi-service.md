---
title: Création d’un service OSGi
description: Créer un service OSGi pour stocker les formulaires à signer
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 1%

---


# Création d’un service OSGi

Le code suivant a été écrit pour stocker les formulaires à signer. Chaque formulaire à signer est associé à un guid unique et à un ID de client. Ainsi, un ou plusieurs formulaires peuvent être associés au même ID de client, mais un GUID unique leur est affecté.

## Interface

Voici la déclaration d’interface utilisée :

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## Insérer des données

La méthode insert data insère une ligne dans la base de données identifiée par la source de données. Chaque ligne de la base de données correspond à un formulaire et est identifiée de manière unique par un GUID et un ID de client. Les données du formulaire et l’URL du formulaire sont également stockées dans cette ligne. La colonne d’état indique si le formulaire a été rempli et signé. La valeur 0 indique que le formulaire doit encore être signé.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## Obtenir les données de formulaire

Le code suivant est utilisé pour récupérer les données de formulaire adaptatif associées au GUID donné. Les données de formulaire sont ensuite utilisées pour préremplir le formulaire adaptatif.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## Mettre à jour le statut de la signature

La fin réussie de la cérémonie de signature déclenche un processus AEM associé au formulaire. La première étape du workflow est une étape de processus qui met à jour l’état de la base de données pour la ligne identifiée par le guide et l’ID de client. Nous définissons également la valeur de l’élément signé dans les données de formulaire sur Y pour indiquer que le formulaire a été rempli et signé. Le formulaire adaptatif sera renseigné avec ces données et la valeur de l’élément de données signé dans les données XML sera utilisée pour afficher le message approprié. Le code updateSignatureStatus est appelé à partir de l’étape de processus personnalisée.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## Obtenir le formulaire suivant à signer

Le code suivant a été utilisé pour obtenir le formulaire suivant pour la signature d’un ID de client donné avec un état 0. Si la requête sql ne renvoie aucune ligne, nous renvoyons la chaîne **&quot;AllDone&quot;** qui indique qu’il n’y a plus de formulaires à signer pour l’ID de client donné.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Assets

Le lot OSGi avec les services mentionnés ci-dessus peut être [téléchargé ici](assets/sign-multiple-forms.jar)