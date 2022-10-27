---
title: Rendu du XDP en PDF avec des droits d’utilisation
description: Application des droits d’utilisation à pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# Rendu du XDP en PDF avec des droits d’utilisation{#rendering-xdp-into-pdf-with-usage-rights}

Un cas d’utilisation courant consiste à effectuer le rendu de xdp dans PDF et à appliquer des extensions Reader au PDF rendu.

Par exemple, dans le portail de formulaires d’AEM Forms, lorsqu’un utilisateur clique sur XDP, nous pouvons effectuer le rendu de XDP en tant que PDF et étendre le PDF.


Pour réaliser ce cas d’utilisation, procédez comme suit.

* Ajoutez le certificat Reader Extensions à l’utilisateur &quot;fd-service&quot;. Les étapes d’ajout d’informations d’identification Reader Extensions sont répertoriées. [here](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=fr)


* Vous pouvez également visionner la vidéo sur [configuration des informations d’identification Reader Extensions](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* Créez un service OSGi personnalisé qui effectue le rendu et applique les droits d’utilisation. Le code permettant d’y parvenir est répertorié ci-dessous.

## Rendu XDP et application des droits d’utilisation {#render-xdp-and-apply-usage-rights}

* Ligne 7 : L’utilisation de renderPDFForm de FormsService génère un PDF à partir du XDP.

* Lignes 8 à 14 : Les droits d’utilisation appropriés sont définis. Ces droits d’utilisation sont récupérés à partir des paramètres de configuration OSGi.

* Ligne 20 : Utilisation du resourceresolver associé au service user fd-service

* Ligne 24 : La méthode secureDocument de DocumentAssuranceService est utilisée pour appliquer les droits d’utilisation.

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

La capture d’écran suivante présente les propriétés de configuration exposées. La plupart des droits d’utilisation courants sont exposés via cette configuration.

![Propriétés de configuration](assets/configurationproperties.gif)

Le code suivant indique le code utilisé pour créer les paramètres de configuration OSGi.

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## Création d’un servlet pour la diffusion en continu du PDF {#create-servlet-to-stream-the-pdf}

L’étape suivante consiste à créer une servlet avec une méthode de GET pour renvoyer le PDF étendu du lecteur à l’utilisateur. Dans ce cas, l’utilisateur est invité à enregistrer le PDF dans son système de fichiers. Cela est dû au fait que le PDF est rendu en tant que PDF dynamique et que les visionneuses pdf fournies avec les navigateurs ne prennent pas en charge les pdf dynamiques.

Voici le code de la servlet. Nous transmettons le chemin d’accès du XDP dans le référentiel CRX à ce servlet.

Nous appelons ensuite la méthode renderAndExtendXdp de com.aemformsséchantillons.documentservices.core.DocumentServices.

Le PDF étendu du lecteur est ensuite diffusé en continu vers l’application appelante.

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

Pour le tester sur votre serveur local, procédez comme suit :
1. [Télécharger et installer le bundle DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Télécharger et installer le bundle AEM FormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [Téléchargement du modèle de portail personnalisé html](assets/render-and-extend-template.zip)
1. [Téléchargez et importez les ressources liées à cet article dans AEM à l’aide du gestionnaire de modules.](assets/renderandextendxdp.zip)
   * Ce module comprend un exemple de portail et de fichier xdp.
1. Ajout d’un certificat Reader Extensions à l’utilisateur &quot;fd-service&quot;
1. Pointez votre navigateur sur [page web du portail](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. Cliquez sur l’icône pdf pour effectuer le rendu du fichier xdp en tant que fichier pdf avec des droits d’utilisation appliqués.
