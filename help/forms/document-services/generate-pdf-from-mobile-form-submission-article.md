---
title: Générer un PDF à partir de l’envoi du formulaire HTML5
description: Générer un PDF à partir de l’envoi du formulaire mobile
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 180
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 100%

---

# Générer un PDF à partir de l’envoi du formulaire HTML5 {#generate-pdf-from-htm-form-submission}

Cet article décrit les étapes à suivre pour générer des PDF à partir d’un envoi du formulaire HTML5 (ou formulaire mobile). Cette démonstration explique également les étapes nécessaires à l’ajout d’une image au formulaire HTML5 et à la fusion de l’image dans le PDF final.


Pour fusionner les données envoyées dans le modèle XDP, procédez comme suit :

Écrivez un servlet pour gérer l’envoi du formulaire HTML5.

* Dans ce servlet, récupérez les données envoyées.
* Fusionnez ces données avec le modèle XDP pour générer le fichier PDF.
* Rediffusez le PDF vers l’application appelante.

Voici le code de servlet qui extrait les données envoyées de la requête. Il appelle ensuite la méthode personnalisée .mobileFormToPDF de documentServices pour obtenir le PDF.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
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
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

Pour ajouter une image au formulaire mobile et l’afficher dans le PDF, nous avons utilisé les éléments suivants :

Modèle XDP : dans le modèle XDP, nous avons ajouté un champ d’image et un bouton appelé btnAddImage. Le code suivant gère l’événement de clic de btnAddImage dans notre profil personnalisé. Comme vous pouvez le constater, nous déclenchons l’événement de clic file1. Aucun codage n’est nécessaire dans le XDP pour réaliser ce cas d’utilisation.

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Profil personnalisé](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). L’utilisation d’un profil personnalisé facilite la manipulation des objets DOM de HTML du formulaire mobile. Un élément de fichier masqué est ajouté au fichier HTML.jsp. Lorsque l’utilisateur ou l’utilisatrice clique sur « Ajouter votre photo », l’événement de clic de l’élément de fichier est déclenché. L’utilisateur ou l’utilisatrice peut ainsi parcourir et sélectionner la photo à joindre. Nous utilisons ensuite l’objet JavaScript FileReader pour obtenir la chaîne codée en base64 de l’image. La chaîne d’image en base64 est stockée dans le champ de texte du formulaire. Lorsque le formulaire est envoyé, nous extrayons cette valeur et l’insérons dans l’élément img du XML. Ce XML est ensuite utilisé pour fusionner avec le XDP afin de générer le PDF final.

Le profil personnalisé utilisé pour cet article a été mis à votre disposition dans le cadre des ressources de cet article.

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

Le code ci-dessus est exécuté lorsque nous déclenchons l’événement de clic de l’élément de fichier. Ligne 5 :extraction du contenu du fichier téléchargé sous la forme d’une chaîne en base64 et stockage dans le champ de texte. Cette valeur est ensuite extraite lorsque le formulaire est envoyé à notre servlet.

Nous configurons ensuite les propriétés suivantes (avancées) de notre formulaire mobile dans AEM.

* URL d’envoi : http://localhost:4502/bin/handlemobileformsubmission. Il s’agit de notre servlet qui fusionnera les données envoyées avec le modèle XDP.
* Profil de rendu de HTML : assurez-vous de sélectionner « AddImageToMobileForm ». Cela déclenche le code permettant d’ajouter une image au formulaire.

Pour tester cette fonctionnalité sur votre propre serveur, procédez comme suit :

* [Déployez le lot AemFormsDocumentServices.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Déployez le lot Developing with Service User.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et installez le package associé à cet article.](assets/pdf-from-mobile-form-submission.zip)

* Assurez-vous que l’URL d’envoi et le profil de rendu de HTML sont correctement définis en affichant la page des propriétés du [XDP](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp).

* [Prévisualiser le XDP au format HTML](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Ajoutez une image au formulaire et envoyez-le. Vous devriez récupérer le PDF avec l’image dedans.
