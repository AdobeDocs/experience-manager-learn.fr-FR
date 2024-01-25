---
title: 'Déclencher le workflow AEM lors de l’envoi d’un formulaire HTML5 : créer un profil personnalisé'
description: Continuez à remplir le formulaire mobile en mode hors ligne et soumettez-le pour déclencher le workflow AEM.
feature: Mobile Forms
doc-type: article
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 125
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 100%

---

# Créer un profil personnalisé

Dans cette partie, nous allons créer un [profil personnalisé.](https://helpx.adobe.com/fr/livecycle/help/mobile-forms/creating-profile.html) Le profil est responsable du rendu du document XDP au format HTML. Un profil par défaut prêt à l’emploi est disponible pour le rendu des documents XDP au format HTML. Il représente une version personnalisée du service de rendu de formulaires mobiles. Vous pouvez utiliser le service de rendu Mobile Forms pour personnaliser l’apparence, le comportement et les interactions des formulaires mobiles. Dans notre profil personnalisé, nous allons capturer les données renseignées dans le formulaire mobile à l’aide de l’API Guidebridge. Ces données sont ensuite envoyées au servlet personnalisé qui génère ensuite un PDF interactif et le diffuse à nouveau vers l’application appelante.

Recueillez les données de formulaire à l’aide de l’API JavaScript `formBridge`. La méthode `getDataXML()` est choisie :

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

Dans la méthode du gestionnaire de succès, nous effectuons un appel au servlet personnalisé qui s’exécute dans AEM. Ce servlet affiche et renvoie un PDF interactif contenant les données du formulaire mobile.

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

## Générer un fichier PDF interactif

Voici le code de servlet responsable du rendu du PDF interactif et du renvoi du PDF à l’application appelante. Le servlet appelle la méthode `mobileFormToInteractivePdf` du service OSGi DocumentServices personnalisé.

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

### Effectuer le rendu d’un fichier PDF interactif

Le code suivant utilise l’[API Forms Service](https://helpx.adobe.com/fr/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) pour effectuer le rendu du PDF interactif avec les données du formulaire mobile.

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

Pour télécharger un PDF interactif à partir d’un formulaire mobile partiellement rempli, [cliquez ici](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content).
Une fois le PDF téléchargé, l’étape suivante consiste à envoyer le PDF et déclencher un workflow AEM. Ce workflow fusionne les données du PDF envoyé et génère un PDF non interactif à des fins de révision.

Le profil personnalisé créé pour ce cas d’utilisation est disponible dans la section Ressources du tutoriel.
