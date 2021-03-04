---
title: Déclencher le processus AEM sur l’envoi de formulaires HTML5
seo-title: Déclencher le flux de travail AEM sur l’envoi de formulaire HTML5
description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
seo-description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---


# Créer un Profil personnalisé

Dans cette partie, nous allons créer un [profil personnalisé.](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) Un profil est responsable du rendu du XDP au format HTML. Un profil par défaut est fourni hors champ pour le rendu des fichiers XDP au format HTML. Il représente une version personnalisée du service Mobile Forms Rendition. Vous pouvez utiliser le service Mobile Form Rendition pour personnaliser l’apparence, le comportement et les interactions de Mobile Forms. Dans notre profil personnalisé, nous capturerons les données renseignées dans le formulaire mobile à l’aide de l’API guidebridge. Ces données sont ensuite envoyées à la servlet personnalisée qui génère ensuite un PDF interactif et le diffuse à nouveau dans l’application appelante.

Récupérez les données de formulaire à l’aide de l’API JavaScript `formBridge`. Nous utilisons la méthode `getDataXML()` :

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

Dans la méthode du gestionnaire de succès, nous faisons un appel à la servlet personnalisée s&#39;exécutant dans AEM. Cette servlet génère et renvoie des fichiers pdf interactifs avec les données du formulaire mobile.

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

Voici le code de servlet responsable du rendu du pdf interactif et du renvoi du pdf à l’application appelante. La servlet appelle la méthode `mobileFormToInteractivePdf` du service OSGi Document Services personnalisé.

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

Le code suivant utilise l’[API du service Forms](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) pour générer un PDF interactif avec les données du formulaire pour périphériques mobiles.

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

Pour vue la possibilité de télécharger un PDF interactif à partir d’un formulaire mobile partiellement rempli, [cliquez ici](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content).
Une fois le fichier PDF téléchargé, l’étape suivante consiste à envoyer le fichier PDF pour déclencher un processus AEM. Ce processus fusionne les données du PDF envoyé et génère un PDF non interactif en révision.

Le profil personnalisé créé pour ce cas d’utilisation est disponible dans le cadre de ces ressources de didacticiel.
