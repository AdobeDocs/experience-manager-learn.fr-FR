---
title: Stocker les données d’un formulaire
description: Stocker les données de formulaire avec le nouveau mappage de pièces jointes dans la base de données
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6538
thumbnail: 6538.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 2bd9fe63-8f42-4b89-95a0-13ade49bc31b
duration: 38
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '75'
ht-degree: 100%

---

# Stocker les données d’un formulaire

L’étape suivante consiste à créer un service pour insérer une nouvelle ligne dans la base de données afin de stocker les données de formulaire adaptatif et les informations sur les pièces jointes associées.
La copie d’écran suivante montre une ligne dans la base de données.


![Exemple de ligne.](assets/sample-row.JPG)


Le code suivant insère une nouvelle ligne dans la base de données avec les données appropriées.

```java
public String storeFormData(String formData, String attachmentsInfo, String telephoneNumber) {
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData + "and the telephone number to insert is  " + telephoneNumber);
    String insertRowSQL = "INSERT INTO aemformstutorial.formdatawithattachments(guid,afdata,attachmentsInfo,telephoneNumber) VALUES(?,?,?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
        pstmt = null;
        pstmt = c.prepareStatement(insertRowSQL);
        pstmt.setString(1, randomUUIDString);
        pstmt.setString(2, formData);
        pstmt.setString(3, attachmentsInfo);
        pstmt.setString(4, telephoneNumber);
        log.debug("Executing the insert statment  " + pstmt.executeUpdate());
        c.commit();
    } catch (SQLException e) {

        log.error("unable to insert data in the table", e.getMessage());
    } finally {
        if (pstmt != null) {
            try {
                pstmt.close();
            } catch (SQLException e) {
                log.debug("error in closing prepared statement " + e.getMessage());
            }
        }
        if (c != null) {
            try {
                c.close();
            } catch (SQLException e) {
                log.debug("error in closing connection " + e.getMessage());
            }
        }
    }
    return randomUUIDString;
}
```

## Étapes suivantes

[Implémenter Enregistrer et quitter](./create-servlet.md)

