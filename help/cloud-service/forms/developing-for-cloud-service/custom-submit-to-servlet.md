---
title: Créer un gestionnaire d’actions d’envoi personnalisé
description: Envoyer un formulaire adaptatif à un gestionnaire d’envoi personnalisé
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
kt: 8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '210'
ht-degree: 100%

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

## Créer un gestionnaire d’envoi personnalisé

Créez votre action d’envoi personnalisée dans le dossier `apps/bankingapplication` de la même manière que dans les [versions antérieures d’AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=fr). Pour les besoins de ce tutoriel, je crée un dossier appelé SubmitToAEMServlet sous le nœud `apps/bankingapplication` dans le référentiel CRX.

Le code suivant dans le fichier post.POST.jsp transfère simplement la demande au servlet monté sur /bin/formstutorial. Il s’agit du même servlet qui a été créé à l’étape précédente.

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

Dans votre projet AEM dans IntelliJ, cliquez sur le dossier `apps/bankingapplication` avec le bouton droit de la souris, puis sélectionnez Nouveau | Package et saisissez SubmitToAEMServlet après apps.bankingapplication dans la boîte de dialogue du nouveau package. Cliquez avec le bouton droit sur le nœud SubmitToAEMServlet et sélectionnez le référentiel. | Obtenir la commande pour synchroniser le projet AEM avec le référentiel de serveur AEM.


## Configurer un formulaire adaptatif

Vous pouvez désormais configurer n’importe quel formulaire adaptatif à envoyer à ce gestionnaire d’envoi personnalisé appelé **Envoyer au servlet AEM**

## Étapes suivantes

[Activer les composants du portail Formulaires](./forms-portal-components.md)
