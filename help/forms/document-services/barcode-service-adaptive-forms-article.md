---
title: Service de code à barres avec formulaires adaptatifs
description: Utilisation du service de code à barres pour décoder le code à barres et remplir les champs de formulaire à partir des données extraites.
feature: Barcoded Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f89cd02d-3ffe-42c6-b547-c0445f912ee8
last-substantial-update: 2020-02-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '354'
ht-degree: 100%

---

# Service de code à barres avec formulaires adaptatifs{#barcode-service-with-adaptive-forms}

Cet article présente l’utilisation du service de code à barres pour renseigner le formulaire adaptatif. Consultez le cas d’utilisation suivant :

1. L’utilisateur ou l’utilisatrice ajoute le PDF avec le code à barres comme pièce jointe de formulaire adaptatif.
1. Le chemin d’accès de la pièce jointe est envoyé au servlet.
1. Le servlet a décodé le code à barres et renvoie les données au format JSON.
1. Le formulaire adaptatif est alors renseigné à l’aide des données décodées.

Le code suivant décode le code à barres et renseigne un objet JSON avec les valeurs décodées. Le servlet renvoie ensuite l’objet JSON dans sa réponse à l’application appelante.



```java
public JSONObject extractBarCode(Document pdfDocument) {
  // TODO Auto-generated method stub
  try {
   org.w3c.dom.Document result = barcodeService.decode(pdfDocument, true, false, false, false, false, false,
     false, false, CharSet.UTF_8);
   List<org.w3c.dom.Document> listResult = barcodeService.extractToXML(result, Delimiter.Carriage_Return,
     Delimiter.Tab, XMLFormat.XDP);
   log.debug("the form1 lenght is " + listResult.get(0).getElementsByTagName("form1").getLength());
   JSONObject decodedData = new JSONObject();
   decodedData.put("name", listResult.get(0).getElementsByTagName("Name").item(0).getTextContent());
   decodedData.put("address", listResult.get(0).getElementsByTagName("Address").item(0).getTextContent());
   decodedData.put("city", listResult.get(0).getElementsByTagName("City").item(0).getTextContent());
   decodedData.put("state", listResult.get(0).getElementsByTagName("State").item(0).getTextContent());
   decodedData.put("zipCode", listResult.get(0).getElementsByTagName("ZipCode").item(0).getTextContent());
   decodedData.put("country", listResult.get(0).getElementsByTagName("Country").item(0).getTextContent());
   log.debug("The JSON Object is " + decodedData.toString());
   return decodedData;
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
 }
```

Le code de servlet se trouve ci-après. Ce servlet est appelé lorsque l’utilisateur ou l’utilisatrice ajoute une pièce jointe au formulaire adaptatif. Le servlet renvoie l’objet JSON à l’application appelante. L’application appelante renseigne ensuite le formulaire adaptatif avec les valeurs extraites de l’objet JSON.

```java
@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/decodebarcode"

})
public class DecodeBarCode extends SlingSafeMethodsServlet {
 @Reference
 DocumentServices documentServices;
 @Reference
 GetResolver getResolver;
 private static final Logger log = LoggerFactory.getLogger(DecodeBarCode.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  ResourceResolver fd = getResolver.getFormsServiceResolver();
  Node pdfDoc = fd.getResource(request.getParameter("pdfPath")).adaptTo(Node.class);
  Document pdfDocument = null;
  log.debug("The path of the pdf I got was " + request.getParameter("pdfPath"));
  try {
   pdfDocument = new Document(pdfDoc.getPath());
   JSONObject decodedData = documentServices.extractBarCode(pdfDocument);
   response.setContentType("application/json");
   response.setHeader("Cache-Control", "nocache");
   response.setCharacterEncoding("utf-8");
   PrintWriter out = null;
   out = response.getWriter();
   out.println(decodedData.toString());
  } catch (RepositoryException | IOException e1) {
   // TODO Auto-generated catch block
   e1.printStackTrace();
  }
 }
}
```

Le code suivant fait partie de la bibliothèque cliente référencée par le formulaire adaptatif. Lorsqu’un utilisateur ou une utilisatrice ajoute la pièce jointe au formulaire adaptatif, ce code est déclenché. Le code effectue un appel GET au servlet avec le chemin d’accès de la pièce jointe transmis dans le paramètre de requête. Les données reçues de l’appel au servlet sont ensuite utilisées pour remplir le formulaire adaptatif.

```javascript
$(document).ready(function()
   {
       guideBridge.on("elementValueChanged",function(event,data){
             if(data.target.name=="fileAttachment")
         {
             window.guideBridge.getFileAttachmentsInfo({
        success:function(list) 
                 {
                     console.log(list[0].name + " "+ list[0].path);
                      const getFormNames = '/bin/decodebarcode?pdfPath='+list[0].path;
      $.getJSON(getFormNames, function (data) {
                            console.log(data);
                            var nameField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Name[0]");
                            nameField.value = data.name;
                            var addressField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Address[0]");
                            addressField.value = data.address;
                            var cityField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].City[0]");
                            cityField.value = data.city;
                            var stateField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].State[0]");
                            stateField.value = data.state;
                             var zipField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Zip[0]");
                            zipField.value = data.zipCode;
                            var countryField = window.guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].Country[0]");
                            countryField.value = data.country;
                        });
                        }
                  });
             }
         });
        });
```

>[!NOTE]
>
>Le formulaire adaptatif inclus dans ce package a été créé à l’aide d’AEM Forms 6.4. Si vous envisagez d’utiliser ce package dans l’environnement AEM Forms 6.3, veuillez créer le formulaire adaptatif dans AEM Forms 6.3.

Ligne 12 - Code personnalisé pour obtenir le résolveur de service. Ce lot est fourni dans le cadre des ressources de ces articles.

Ligne 23 - Appelez la méthode extractBarCode de Document Services pour que l’objet JSON soit renseigné avec des données décodées.

Pour lancer cette opération sur votre système, veuillez procéder comme suit :

1. [Téléchargez BarcodeService.zip](assets/barcodeservice.zip) et importez-le dans AEM à l’aide du gestionnaire de packages.
1. [Téléchargez et installez le lot Document Services personnalisé.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Téléchargez et installez le lot DevelopingWithServiceUser.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Téléchargez l’exemple de formulaire PDF.](assets/barcode.pdf)
1. Pointez votre navigateur sur l’[exemple de formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled).
1. Téléchargez l’exemple de PDF fourni.
1. Vous devriez alors voir les formulaires renseignés avec les données.
