---
title: Création d’un gestionnaire d’action d’envoi personnalisé
description: Envoi d’un formulaire adaptatif à un gestionnaire d’envoi personnalisé
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8852
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Créer un servlet pour traiter les données envoyées

Lancez votre projet aem-banking dans IntelliJ.
Créez un servlet simple pour générer les données envoyées dans le fichier journal. Assurez-vous que le code se trouve dans le projet principal, comme illustré dans la capture d’écran ci-dessous.
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## Créer un envoi personnalisé

Créez votre envoi personnalisé dans le dossier application/application bancaire de la même manière que vous le feriez dans le dossier [versions antérieures d’AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
Le code suivant dans le fichier post.POST.jsp transfère simplement la demande au servlet monté sur /bin/formstutorial. Il s’agit du même servlet qui a été créé à l’étape précédente.

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## Configurer un formulaire adaptatif

Vous pouvez maintenant configurer votre formulaire adaptatif pour l’envoyer à ce gestionnaire d’envoi personnalisé appelé **Envoyer au servlet AEM**



