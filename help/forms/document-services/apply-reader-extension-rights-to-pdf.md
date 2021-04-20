---
title: Rendu du fichier XDP au format PDF avec des droits d’utilisation
seo-title: Rendu du fichier XDP au format PDF avec des droits d’utilisation
description: Appliquer les droits d’utilisation au format pdf
seo-description: Appliquer les droits d’utilisation au format pdf
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 12%

---


# Application des extensions de Reader

Reader Extensions permet de manipuler les droits d’utilisation sur les documents PDF. Les droits d’utilisation correspondent à une fonctionnalité disponible dans Acrobat , mais non dans Adobe Reader. La fonctionnalité contrôlée par les extensions de Reader permet d’ajouter des commentaires à un document, de remplir des formulaires et d’enregistrer le document. Les documents PDF dotés de droits d’utilisation sont appelés des documents dont les droits sont activés. Un utilisateur qui ouvre un document PDF dont les droits sont activés dans Adobe Reader peut effectuer les opérations qui sont autorisées pour ce document.
Pour tester cette fonctionnalité, vous pouvez essayer ce [lien](https://forms.enablementadobe.com/content/samples/samples.html?query=0). L’exemple de nom est &quot;Render XDP with RE&quot;.

Pour ce faire, nous devons procéder comme suit :
* Ajoutez le certificat Reader Extensions à l’utilisateur &quot;fd-service&quot;. Les étapes permettant d’ajouter des informations d’identification d’extensions de Reader sont répertoriées [ici](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* Créez un service OSGi personnalisé qui appliquera des droits d’utilisation aux documents. Le code pour y parvenir est répertorié ci-dessous.

```java
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service=ApplyUsageRights.class,immediate = true)
public class ApplyUsageRights implements ReaderExtendPDF {
@Reference
DocAssuranceService docAssuranceService;
@Reference
GetResolver getResolver;
@Override
public Document applyUsageRights(Document pdfDocument,UsageRights usageRights) {
      ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
      UnlockOptions unlockOptions = null;
      ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
      reOptions.setCredentialAlias("ares");
      reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
      reOptions.setReOptions(reOptionsSpec);
    try {
          return docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
          unlockOptions);
        } catch (Exception e) {
            e.printStackTrace();
        }
    return null;
}

}
```

## Créer un servlet pour diffuser en continu le fichier PDF {#create-servlet-to-stream-the-pdf}

L’étape suivante consiste à créer une servlet avec une méthode de POST pour renvoyer le PDF étendu Reader à l’utilisateur. Dans ce cas, l’utilisateur est invité à enregistrer le fichier PDF dans son système de fichiers. En effet, le PDF est rendu au format PDF dynamique et les visionneuses PDF fournies avec les navigateurs ne gèrent pas les fichiers PDF dynamiques.

Voici le code de la servlet. La servlet sera appelée à partir de l&#39;action **customsubmit** du formulaire adaptatif.
Servlet crée l’objet UsageRights et lui définit les propriétés en fonction des valeurs saisies par l’utilisateur dans le formulaire adaptatif. La servlet appelle ensuite la méthode **applyUsageRights** du service créé à cet effet.

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Enumeration;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.PathNotFoundException;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFormatException;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = Servlet.class, property = {
sling.servlet.methods=get", "sling.servlet.paths=/bin/applyrights"
})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {
  @Reference
  GetResolver getResolver;
  @Reference
  ApplyUsageRights applyRights;
  public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
  try {
        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
        InputSource is = new InputSource();
        is.setCharacterStream(new StringReader(xmlString));
  return db.parse(is);
} catch (ParserConfigurationException e) {
    e.printStackTrace();
} catch (SAXException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}
return null;
}
@Override
protected void doGet(SlingHttpServletRequest request,SlingHttpServletResponse response)
{
  doPost(request,response);
}
@Override
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  UsageRights usageRights = new UsageRights();
  String submittedData = request.getParameter("jcr:data");
  org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
  String formFill = submittedXml.getElementsByTagName("formfill").item(0).getTextContent();
  usageRights.setEnabledFormDataImportExport(true);
  usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
  usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
  usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
  usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
  String attachmentPath = submittedXml.getElementsByTagName("attachmentpath").item(0).getTextContent();
  Node pdfNode = getResolver.getFormsServiceResolver().getResource(attachmentPath).adaptTo(Node.class);
  javax.jcr.Node jcrContent = null;
try {
    jcrContent = pdfNode.getNode("jcr:content");
    } catch (RepositoryException e1) {
      e1.printStackTrace();
    }

try {
    InputStream is = jcrContent.getProperty("jcr:data").getBinary().getStream();
    Session jcrSession = getResolver.getFormsServiceResolver().adaptTo(Session.class);
    Document documentToReaderExtend = new Document(is);
    documentToReaderExtend =  applyRights.applyUsageRights(documentToReaderExtend,usageRights);
    documentToReaderExtend.copyToFile(new File("c:\\scrap\\doctore.pdf"));
    InputStream fileInputStream = documentToReaderExtend.getInputStream();
    documentToReaderExtend.close();
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
  } catch (ValueFormatException e) {
      e.printStackTrace();
  } catch (PathNotFoundException e) {
      e.printStackTrace();
  } catch (RepositoryException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

}

}
```

Pour tester cette fonctionnalité sur votre serveur local, procédez comme suit :
1. [Téléchargement et installation du lot DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Téléchargez et installez le lot](assets/ares.ares.core-ares.jar) ares.ares.core-ares. Le service personnalisé et la servlet lui permettent d’appliquer des droits d’utilisation et de diffuser en continu le pdf.
1. [Importer les libs du client et l’envoi personnalisé](assets/applyaresdemo.zip)
1. [Importation du formulaire adaptatif](assets/applyaresform.zip)
1. Ajouter le certificat Reader Extensions à l’utilisateur &quot;fd-service&quot;
1. [Prévisualisation de formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. Sélectionner les droits appropriés et télécharger le fichier PDF
1. Cliquez sur Envoyer pour obtenir le PDF étendu Reader.



