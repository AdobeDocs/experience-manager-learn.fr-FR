---
title: Service De Codes À Barres Avec Forms Adaptatif
seo-title: Service De Codes À Barres Avec Forms Adaptatif
description: Utilisation du service de code à barres pour décoder le code à barres et remplir les champs de formulaire à partir des données extraites
seo-description: Utilisation du service de code à barres pour décoder le code à barres et remplir les champs de formulaire à partir des données extraites
uuid: 42568b81-cbcd-479e-8d9a-cc0b244da4ae
feature: Barcoded-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 1224de6d-7ca1-4e9d-85fe-cd675d03e262
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---


# Service De Codes À Barres Avec Forms Adaptative{#barcode-service-with-adaptive-forms}

Cet article montre l&#39;utilisation du service de codes à barres pour renseigner le formulaire adaptatif. Le cas d’utilisation est le suivant :

1. L’utilisateur ajoute un fichier PDF avec un code à barres en tant que pièce jointe de formulaire adaptatif.
1. Le chemin d&#39;accès de la pièce jointe est envoyé à la servlet
1. La servlet a décodé le code à barres et renvoie les données au format JSON.
1. Le formulaire adaptatif est ensuite renseigné à l’aide des données décodées.

Le code suivant décode le code à barres et renseigne un objet JSON avec les valeurs décodées. La servlet renvoie ensuite l’objet JSON dans sa réponse à l’application appelante.

Vous pouvez voir cette fonctionnalité en direct, consultez le [portail d’exemples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) et recherchez la démonstration du service de codes à barres.

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

Voici le code de servlet. Cette servlet est appelée lorsque l’utilisateur ajoute une pièce jointe au formulaire adaptatif. La servlet renvoie l’objet JSON à l’application appelante. L’application appelante remplit ensuite le formulaire adaptatif avec les valeurs extraites de l’objet JSON.

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

Le code suivant fait partie de la bibliothèque cliente référencée par le formulaire adaptatif. Lorsqu’un utilisateur ajoute la pièce jointe au formulaire adaptatif, ce code est déclenché. Le code effectue un appel de GET à la servlet avec le chemin de la pièce jointe transmis dans le paramètre de requête. Les données reçues de l’appel de servlet sont ensuite utilisées pour remplir le formulaire adaptatif.

```
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
>Le formulaire adaptatif inclus dans ce package a été créé à l&#39;aide d&#39;AEM Forms 6.4. Si vous envisagez d&#39;utiliser ce package dans l&#39;environnement AEM Forms 6.3, créez le formulaire adaptatif dans AEM formulaire 6.3.

Ligne 12 - Code personnalisé pour obtenir le résolveur de service. Ce lot est inclus dans les actifs de cet article.

Ligne 23 - Appelez la méthode extractBarCode de Document Services pour que l’objet JSON soit renseigné avec des données décodées.

Pour exécuter cette opération sur votre système, suivez les étapes ci-après.

1. [Téléchargement de BarcodeService.](assets/barcodeservice.zip) zipet importation dans AEM à l’aide du gestionnaire de packages
1. [Téléchargement et installation de l’offre groupée Document Services personnalisée](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [Téléchargement et installation du lot DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [Téléchargement de l’exemple de formulaire PDF](assets/barcode.pdf)
1. Pointez votre navigateur sur l’[exemple de formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/barcodedemo/jcr:content?wcmmode=disabled)
1. Télécharger l’exemple de PDF fourni
1. Vous devriez voir les formulaires renseignés avec les données

