---
title: Stockage et récupération des données de formulaire de la base de données MySQL - Servlet pour stocker des données de formulaire
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes impliquées dans le stockage et la récupération des données de formulaire
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: dd82f309-dd4e-42ce-8856-e51c898024f5
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '95'
ht-degree: 0%

---

# Servlet de stockage des données de formulaire

L’étape suivante consiste à créer un servlet qui insère ou met à jour les données de formulaire. Le servlet appelle les méthodes appropriées du service OSGi pour insérer ou mettre à jour la base de données. Les données de formulaire adaptatif stockées sont associées à un GUID. Le même GUID est ensuite utilisé pour mettre à jour les données de formulaire. Cette servlet est appelée lorsque l’utilisateur clique sur le bouton &quot;SaveAndContinueLater&quot;.

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
