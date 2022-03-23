---
title: Déclencher AEM processus sur l’envoi de formulaire HTM5 - Créer un profil personnalisé
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Créer un profil personnalisé

Dans cette partie, nous allons créer une [profil personnalisé.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un profil est responsable du rendu du XDP en tant que HTML. Un profil par défaut est fourni prêt à l’emploi pour le rendu des XDP en tant que HTML. Il représente une version personnalisée du service de rendu Forms Mobile. Vous pouvez utiliser le service de rendu de formulaire Mobile pour personnaliser l’apparence, le comportement et les interactions de Mobile Forms. Dans notre profil personnalisé, nous allons capturer les données renseignées dans le formulaire mobile à l’aide de l’API guidebridge. Ces données sont ensuite envoyées au servlet personnalisé qui génère ensuite un PDF interactif et le diffuse à nouveau vers l’application appelante.

Obtention des données de formulaire à l’aide du `formBridge` API JavaScript. Nous utilisons les `getDataXML()` method :

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

Dans la méthode du gestionnaire de succès, nous effectuons un appel au servlet personnalisé qui s’exécute dans AEM. Ce servlet affiche et renvoie un pdf interactif avec les données du formulaire mobile.

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Générer un PDF interactif

Voici le code de servlet responsable du rendu du pdf interactif et du renvoi du pdf à l’application appelante. Le servlet appelle `mobileFormToInteractivePdf` du service OSGi Document Services personnalisé.

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
          response.setContentType("application/pdf");
          response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
          response.setContentLength((int) fileInputStream.available());
          OutputStream responseOutputStream = response.getOutputStream();
          int bytes;
          while ((bytes = fileInputStream.read()) != -1) {
              responseOutputStream.write(bytes);
          }
          responseOutputStream.flush();
          responseOutputStream.close();
      } catch (IOException e) {
        // TODO Add proper error logging
      }
    }
}
```

### Render Interactive PDF

Le code suivant utilise la variable [API Forms Service](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) pour rendre le PDF interactif avec les données du formulaire mobile.

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

Pour afficher la possibilité de télécharger un PDF interactif à partir d’un formulaire mobile partiellement rempli, [cliquez ici](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Une fois le PDF téléchargé, l’étape suivante consiste à envoyer le PDF pour déclencher un workflow d’AEM. Ce workflow fusionnera les données du PDF envoyé et génèrera un PDF non interactif à des fins de révision.

Le profil personnalisé créé pour ce cas d’utilisation est disponible dans le cadre de ces ressources de tutoriel.
