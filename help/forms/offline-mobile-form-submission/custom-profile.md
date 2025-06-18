---
title: Déclencher le workflow AEM lors de l’envoi d’un formulaire HTML5 - Création d’un profil personnalisé
description: Création d’un profil personnalisé pour télécharger un PDF interactif avec les données du formulaire HTML5 partiellement rempli
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '244'
ht-degree: 100%

---

# Création d’un profil personnalisé

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
    let postURL ="/bin/generateinteractivepdf";
    console.log("The data: " + data);
    xhr.open('POST',postURL);
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    let parts = window.location.pathname.split("/");
    let formName = parts[parts.length-2];
    const updatedFilename = formName.replace(/\.xdp$/, '.pdf');

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
                a.download = updatedFilename;
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## Générer un fichier PDF interactif

Voici le code de servlet responsable du rendu du PDF interactif et du renvoi du PDF à l’application appelante. Le servlet appelle la méthode `mobileFormToInteractivePdf` du service OSGi DocumentServices personnalisé.

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import java.io.*;
import java.nio.charset.StandardCharsets;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/generateInteractivePDF"})
public class GeneratePDFFromMobileFormData extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    GeneratePDFFromMobileForm generatePDFFromMobileForm;

    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) throws IOException {
        String dataXml = request.getParameter("formData");
        logger.debug("The data is "+dataXml);
        InputStream inputStream = new ByteArrayInputStream(dataXml.getBytes(StandardCharsets.UTF_8));
        Document submittedXml = new Document(inputStream);
       Document interactivePDF =  generatePDFFromMobileForm.generateInteractivePDF(submittedXml,request.getParameter("xdpPath"));
        interactivePDF.copyToFile(new File("interactive.pdf"));
        try {
            InputStream fileInputStream = interactivePDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
            response.setContentLength(fileInputStream.available());
            ServletOutputStream responseOutputStream = response.getOutputStream();

            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }

            responseOutputStream.flush();
            responseOutputStream.close();
        }
        catch(Exception e)
        {
            logger.debug(e.getMessage());
        }


    }
}
```

### Effectuer le rendu d’un fichier PDF interactif

Le code suivant utilise l’[API Forms Service](https://helpx.adobe.com/fr/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) pour effectuer le rendu du PDF interactif avec les données du formulaire mobile.

```java
package com.aemforms.mobileforms.core.documentservices.impl;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.AcrobatVersion;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.adobe.fd.forms.api.PDFFormRenderOptions;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(service = GeneratePDFFromMobileForm.class, immediate = true)
public class GeneratePDFFromMobileFormImpl implements GeneratePDFFromMobileForm {
    @Reference
    FormsService formsService;
    private   transient Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public Document generateInteractivePDF(Document xmlData, String xdpPath)
    {
        String uri = "crx:///content/dam/formsanddocuments";
        String xdpName = xdpPath.substring(31, xdpPath.lastIndexOf("/jcr:content"));
        log.debug("####In mobile form to interactive pdf####   " + xdpName);
        PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
        renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
        renderOptions.setContentRoot(uri);
        Document interactivePDF = null;
        try
        {
            interactivePDF = this.formsService.renderPDFForm(xdpName, xmlData, renderOptions);
        }
        catch (FormsServiceException e)
        {
            log.error(e.getMessage());
        }

        return interactivePDF;

    }
}
```

## Étapes suivantes

[Gestion de l’envoi du formulaire](./handle-form-submission.md)
