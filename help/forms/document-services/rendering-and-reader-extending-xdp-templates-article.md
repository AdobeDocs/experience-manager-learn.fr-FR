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
feature: Forms-service
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---


# Rendu XDP au format PDF avec des droits d’utilisation{#rendering-xdp-into-pdf-with-usage-rights}

Un cas d’utilisation courant consiste à rendre xdp au format PDF et à appliquer des extensions de Reader au fichier PDF rendu.

Par exemple, dans le portail de formulaires d’AEM Forms, lorsqu’un utilisateur clique sur XDP, nous pouvons effectuer le rendu du fichier XDP au format PDF et étendre le fichier PDF au lecteur.

Pour tester cette fonctionnalité, vous pouvez essayer ce [lien](https://forms.enablementadobe.com/content/samples/samples.html?query=0). L’exemple de nom est &quot;Render XDP with RE&quot;.

Pour ce cas d&#39;utilisation, nous devons procéder comme suit.

* Ajoutez le certificat Reader Extensions à l’utilisateur &quot;fd-service&quot;. Les étapes permettant d’ajouter des informations d’identification d’extensions de Reader sont répertoriées [ici](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* Créez un service OSGi personnalisé qui affichera et appliquera des droits d’utilisation. Le code pour y parvenir est répertorié ci-dessous.

## Rendu XDP et application des droits d’utilisation {#render-xdp-and-apply-usage-rights}

* Ligne 7 : L’utilisation du formulaire renderPDFForm de FormsService génère un fichier PDF à partir du fichier XDP.

* Lignes 8 à 14 : Les droits d’utilisation appropriés sont définis. Ces droits d’utilisation sont extraits des paramètres de configuration OSGi.

* Ligne 20 : Utiliser le resourceresolver associé au service utilisateur fd-service

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

La capture d&#39;écran suivante présente les propriétés de configuration exposées. La plupart des droits d’utilisation courants sont exposés via cette configuration.

![](assets/configurationproperties.gif)

Le code suivant montre le code utilisé pour créer les paramètres de configuration OSGi.

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

## Créer un servlet pour diffuser en continu le fichier PDF {#create-servlet-to-stream-the-pdf}

L’étape suivante consiste à créer une servlet avec une méthode de GET pour renvoyer le PDF étendu Reader à l’utilisateur. Dans ce cas, l’utilisateur est invité à enregistrer le fichier PDF dans son système de fichiers. En effet, le PDF est rendu au format PDF dynamique et les visionneuses PDF fournies avec les navigateurs ne gèrent pas les fichiers PDF dynamiques.

Voici le code de la servlet. Nous transmettons le chemin d’accès du fichier XDP dans le référentiel CRX à cette servlet.

Ensuite, nous appelons la méthode renderAndExtendXdp de com.aemformsséchantillons.documentservices.core.DocumentServices.

Le PDF étendu Reader est ensuite diffusé en flux continu dans l’application appelante.

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

Pour tester cette fonctionnalité sur votre serveur local, suivez les étapes ci-après.
1. [Téléchargement et installation du lot DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Téléchargement et installation du lot AEMFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Téléchargez et importez les ressources liées à cet article dans AEM à l’aide de Package Manager.](assets/renderandextendxdp.zip)
   * Ce package contient un exemple de portail et de fichier xdp
1. Ajouter le certificat Reader Extensions à l’utilisateur &quot;fd-service&quot;
1. Pointez votre navigateur vers [la page Web du portail](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html).
1. Cliquez sur l’icône pdf pour afficher le fichier xdp et obtenir le fichier pdf qui est Reader Extended.



