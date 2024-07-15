---
title: AEM Forms avec schéma et données JSON [Partie 2]
description: Il s’agit d’un tutoriel en plusieurs parties consacré à la création de formulaires adaptatifs avec les schémas JSON et à l’interrogation des données envoyées.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
duration: 110
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 100%

---

# Stocker les données envoyées dans la base de données


>[!NOTE]
>
>Il est recommandé d’utiliser la base de données MySQL 8, car elle prend en charge le type de données JSON. Vous devez également installer le pilote approprié pour MySQL DB. J’ai utilisé le pilote disponible à cet emplacement : https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12.

Pour stocker les données envoyées dans la base de données, nous allons écrire un servlet afin d’extraire les données liées, le nom du formulaire et le magasin. Consultez le code complet pour gérer l’envoi du formulaire et stocker afBoundData dans la base de données ci-dessous.

Nous avons créé un envoi personnalisé pour gérer l’envoi du formulaire. Dans le fichier post.POST.jsp de cet envoi personnalisé, nous transférons la demande à notre servlet.

Pour en savoir plus sur les envois personnalisés, consulez cet [article](https://helpx.adobe.com/fr/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html).

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionpool](assets/connectionpooled.gif)

Pour que cette opération fonctionne sur votre système, procédez comme suit :

* [Téléchargez et décompressez le fichier zip.](assets/aemformswithjson.zip)
* Créez un formulaire adaptatif avec un schéma JSON. Vous pouvez utiliser le schéma JSON fourni dans la section Ressources de cet article. Assurez-vous que l’action d’envoi du formulaire est correctement configurée. L’action d’envoi doit être configurée sur « CustomSubmitHelpx ».
* Créez un schéma dans votre instance MySQL en important le fichier schema.sql à l’aide de l’outil MySQL Workbench. Le fichier schema.sql est également fourni dans les ressources du tutoriel.
* Configurez la source de données mise en pool de la connexion Apache Sling à partir de la console web Felix.
* Veillez à nommer votre source de données « aemformswithjson ». Il s’agit du nom utilisé par l’exemple de lot OSGi qui vous est fourni.
* Consultez l’image ci-dessus pour connaître les propriétés. Cela suppose l’utilisation de MySQL comme base de données.
* Déployez le ou les lots OSGi fournis dans les ressources de cet article.
* Prévisualisez le formulaire et envoyez-le.
* Les données JSON sont stockées dans la base de données créée lors de l’import du fichier « schema.sql ».
