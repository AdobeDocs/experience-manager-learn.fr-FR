---
title: Utilisation de l’API GuideBridge pour envoyer des données de formulaire
description: Découvrez comment accéder aux données de formulaire et les envoyer à l’aide de l’API GuideBridge pour les formulaires adaptatifs. Enregistrez et récupérez facilement les données de formulaire.
duration: 68
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-15286
last-substantial-update: 2024-04-05T00:00:00Z
exl-id: 099aaeaf-2514-4459-81a7-2843baa1c981
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '132'
ht-degree: 100%

---


# Accès et envoi de données de formulaire avec l’API GuideBridge

Découvrez comment utiliser l’API GuideBridge pour accéder aux données de formulaire et les envoyer à un point d’entrée REST pour le stockage et la récupération. Cette fonctionnalité permet aux utilisateurs et utilisatrices d’enregistrer et de reprendre facilement la saisie du formulaire.

Les données du formulaire sont enregistrées en déclenchant une fonction JavaScript lors d’un clic sur un bouton dans l’éditeur de règles.

![Éditeur de règles](assets/rule-editor.png)

La fonction JavaScript ci-dessous montre comment envoyer les données de formulaire au point d’entrée spécifié :

```javascript
/**
* Submits data and attachments 
* @name submitFormDataAndAttachments Submit form data and attachments to REST endpoint
* @param {string} endpoint in String format
* @return {string} 
 */
 
function submitFormDataAndAttachments(endpoint) {
    guideBridge.getFormDataObject({
        success: function(resultObj) {
            const afFormData = resultObj.data.data;
            const formData = new FormData();
            formData.append("dataXml", afFormData);
            resultObj.data.attachments.forEach(attachment => {
                console.log(attachment.name);
                formData.append(attachment.name, attachment.data);
            });
            fetch(endpoint, {
                method: 'POST',
                body: formData
            })
            .then(response => {
                if (response.ok) {
                    console.log("Successfully saved");
                    const fld = guideBridge.resolveNode("$form.confirmation");
                    return "Form data was saved successfully";
                } else {
                    throw new Error('Failed to save form data');
                }
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    });
}
```

## Code côté serveur

Le code Java côté serveur suivant gère le traitement des données de formulaire. Ce servlet Java dans AEM est appelé via un appel XHR dans la fonction JavaScript ci-dessus.

```java
package com.azuredemo.core.servlets;

import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.IOException;
import java.io.Serializable;
import java.util.List;

@Component(
   service = {
      Servlet.class
   }
)
@SlingServletResourceTypes(
   resourceTypes = "azure/handleFormSubmission",
   methods = "POST",
   extensions = "json"
)
public class StoreFormSubmission extends SlingAllMethodsServlet implements Serializable {
   private static final long serialVersionUID = 1L;
   private final transient Logger log = LoggerFactory.getLogger(this.getClass());

   protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
      List<RequestParameter> listOfRequestParameters = request.getRequestParameterList();
      log.debug("The size of the list is " + listOfRequestParameters.size());
      
      for (int i = 0; i < listOfRequestParameters.size(); i++) {
         RequestParameter requestParameter = listOfRequestParameters.get(i);
         log.debug("Is this request parameter a form field?" + requestParameter.isFormField());
         
         if (!requestParameter.isFormField()) {
            Document attachmentDOC = new Document(requestParameter.getInputStream());
            attachmentDOC.copyToFile(new File(requestParameter.getName()));
         } else {
            log.debug("Not a form field " + requestParameter.getName());
            log.debug(requestParameter.getString());
         }
      }
      
      response.setStatus(HttpServletResponse.SC_OK);
   }
}
```
