---
title: Stocker et récupérer les données de formulaire à partir d’un servlet de la base de données MySQL pour stocker des données de formulaire
description: Tutoriel en plusieurs parties détaillant les étapes impliquées dans le stockage et la récupération des données de formulaire
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: dd82f309-dd4e-42ce-8856-e51c898024f5
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '95'
ht-degree: 100%

---

# Servlet de stockage des données de formulaire

L’étape suivante consiste à créer un servlet qui insère ou met à jour les données de formulaire. Le servlet appelle les méthodes appropriées du service OSGi pour insérer ou mettre à jour la base de données. Les données de formulaire adaptatif stockées sont associées à un GUID. Le même GUID est ensuite utilisé pour mettre à jour les données de formulaire. Ce servlet est appelé lorsque l’utilisateur ou l’utilisatrice clique sur le bouton « SaveAndContinueLater ».

```java
package com.aemforms.saveandcontinue.core.servlets;

import java.io.IOException;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.aemforms.saveandcontinue.core.FetchStoredFormData;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
},
property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdata"
})
public class StoreDataInDB extends SlingAllMethodsServlet {
  private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
  private static final long serialVersionUID = 1L;
  @Reference
  FetchStoredFormData fetchStoredFormData;
  protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    log.debug("Inside my save af data servlet");
    if (request.getParameter("operation").equalsIgnoreCase("update")) {
      log.debug("The operation is update");
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.updateData(request.getParameter("guid"), request.getParameter("formdata"));
      log.debug("The guid I got was  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
      }
    }

    if (request.getParameter("operation").equalsIgnoreCase("insert")) {
      log.debug("The data I got was " + request.getParameter("formdata"));
      String guid = fetchStoredFormData.storeFormData(request.getParameter("formdata"));
      log.debug("The guid on inserting data  " + guid);
      JsonObject jsonResponse = new JsonObject();
      try {
        jsonResponse.addProperty("guid", guid);
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write(jsonResponse.toString());

      } catch(IOException e) {
        // TODO Auto-generated catch block
        log.debug("error in writing response  " + e.getMessage());
      }
    }

  }

}
```
