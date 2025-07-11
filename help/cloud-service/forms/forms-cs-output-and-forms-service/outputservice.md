---
title: Générer des documents PDF à l’aide du service Output
description: Fusionner les données avec le modèle XDP pour générer un PDF non interactif
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '207'
ht-degree: 100%

---

# Générer des documents PDF à l’aide du service Output

Le [service Output](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html) est un service OSGi qui fait partie d’AEM Document Services. Il prend en charge divers formats et fonctions de conception de sortie d’AEM Forms Designer. Le service Output convertit des modèles XFA et des données XML pour générer des documents d’impression dans différents formats.

Le service Output d’AEM Forms as a Cloud Service ressemble de près à celui d’AEM Forms 6.5. Par conséquent, si vous maîtrisez l’utilisation du service Output dans AEM Forms 6.5, la transition vers AEM Forms as a Cloud Service doit être simple.

Avec le service Output, vous pouvez créer des applications qui vous permettent d’effectuer les opérations suivantes :

+ Générer des documents de formulaire définitifs en complétant des fichiers de modèle avec des données XML.
+ Générer des formulaires de sortie dans différents formats, y compris des flux d’impression PDF non interactifs, PostScript, PCL et ZPL.
+ Générer des fichiers PDF d’impression à partir de fichiers PDF de formulaire XFA.
+ Générez des documents PDF, PostScript, PCL et ZPL en blocs en fusionnant plusieurs jeux de données avec les modèles fournis.

Ce service est conçu pour être utilisé dans le contexte d’une instance AEM Forms as a Cloud Service. Le fragment de code suivant génère un document PDF dans un servlet à l’aide de `OutputService`.

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
