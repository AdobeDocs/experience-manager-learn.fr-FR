---
title: Enregistrer et récupérer des brouillons de lettres
description: Découvrez comment enregistrer et récupérer des brouillons de lettres
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: dc6f64a0-7059-4392-9c29-e66bdef4fd4d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 100%

---

# Enregistrer et récupérer des brouillons de lettres

Le code suivant permet d’enregistrer l’instance de lettre. Les métadonnées de l’instance de lettre sont stockées dans le tableau _icdraft_. Une chaîne unique (draftID) est générée et renvoyée. Cette chaîne unique est ensuite utilisée pour récupérer l’instance de lettre enregistrée.

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## Obtenir la lettre

Le code suivant permet de récupérer le brouillon de lettre enregistré.
Pour charger des instances de lettre enregistrées, vous devez fournir l’ID de brouillon draftID. Des requêtes basées sur l’ID de brouillon draftID sont ensuite envoyées à la base de données afin d’obtenir les métadonnées supplémentaires sur la lettre. Le même ID de brouillon draftID est utilisé pour créer les données de la lettre en lisant le fichier XML approprié à partir du système de fichiers. Un objet CCRDocumentInstance est alors créé et renvoyé.


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### Mettre à jour la lettre

Le code suivant permet de mettre à jour l’instance de lettre enregistrée. Les données de la lettre mise à jour sont écrites dans le système de fichiers à l’aide de l’ID de lettre.

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
        Document icData = letterInstanceToUpdate.getData();
        String draftID = letterInstanceToUpdate.getId();
        log.debug("updating letter instance with draft id =  "+draftID);
        try
            {
                icData.copyToFile(new File(draftID+".xml"));
            } 
        catch (IOException e)
            {
                log.debug("Error updating "+e.getMessage());;
            }
        
    }
```

### Obtenir toutes les lettres enregistrées

AEM Forms ne dispose d’aucune interface utilisateur prête à l’emploi pour répertorier les lettres enregistrées. Dans le cadre de cet article, les instances de lettre enregistrées sont répertoriées au format tabulaire à l’aide d’un formulaire adaptatif.
Vous pouvez personnaliser la requête pour récupérer les instances de lettre enregistrées. Dans cet exemple, la requête porte sur une instance de lettre enregistrée par « admin ».

```java
    public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
      String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
      Connection connection = getConnection();
      Statement statement = null;
      String documentID = "";
      List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
      String draftID;
      String savedInstanceName = "";
      try {
        statement = connection.createStatement();
        ResultSet rs = statement.executeQuery(selectStatement);
        while (rs.next()) {
          documentID = rs.getString("documentID");
          draftID = rs.getString("draftID");
          savedInstanceName = rs.getString("name");
          Document draftData = new Document(new File(draftID + ".xml"));
          CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
          listOfDrafts.add(draftLetter);
        }
      } catch (SQLException e) {
        log.debug("The error is " + e.getMessage());
      } finally {
        if (statement != null) {
          try {
            statement.close();
          } catch (SQLException e) {
            log.debug("error in closing statement" + e.getMessage());
          }
        }
        if (connection != null) {
          try {
            connection.close();
          } catch (SQLException e) {
            log.debug("error in closing connection" + e.getMessage());
          }
        }
      }

      return listOfDrafts;
    }
```

### Projet Eclipse

Le projet Eclipse avec l’exemple de mise en œuvre peut être [téléchargé ici](assets/icdrafts-eclipse-project.zip).
