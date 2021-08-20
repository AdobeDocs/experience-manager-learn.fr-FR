---
title: Générer un fichier PDF à partir de l’envoi de formulaire HTM5
description: Générer un fichier PDF à partir de l’envoi de formulaire mobile
feature: Mobile Forms
version: 6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%

---


# Générer un fichier PDF à partir de l’envoi de formulaire HTM5 {#generate-pdf-from-htm-form-submission}

Cet article décrit les étapes à suivre pour générer des fichiers PDF à partir d’un envoi de formulaire HTML5 (ou Mobile Forms). Cette démonstration explique également les étapes nécessaires à l’ajout d’une image au formulaire HTML5 et à la fusion de l’image dans le PDF final.

Pour voir une démonstration en direct de cette fonctionnalité, consultez l’[exemple de serveur](https://forms.enablementadobe.com/content/samples/samples.html?query=0) et recherchez &quot;Mobile Form To PDF&quot;.

Pour fusionner les données envoyées dans le modèle xdp, procédez comme suit :

Écrire un servlet pour gérer l’envoi du formulaire HTML5

* Dans ce servlet, récupérez les données envoyées.
* Fusionner ces données avec le modèle xdp pour générer le fichier pdf.
* Rediffuser le pdf vers l’application appelante

Voici le code de servlet qui extrait les données envoyées de la requête. Il appelle ensuite la méthode personnalisée documentServices .mobileFormToPDF pour obtenir le pdf.

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

Pour ajouter une image au formulaire mobile et l’afficher dans le pdf, nous avons utilisé les éléments suivants :

Modèle XDP : dans le modèle xdp, nous avons ajouté un champ d’image et un bouton appelé btnAddImage. Le code suivant gère l’événement click de btnAddImage dans notre profil personnalisé. Comme vous pouvez le constater, nous déclenchons l’événement de clic file1. Aucun codage n’est nécessaire dans le xdp pour réaliser ce cas d’utilisation.

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Profil personnalisé](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). L’utilisation d’un profil personnalisé facilite la manipulation des objets DOM HTML du formulaire mobile. Un élément de fichier masqué est ajouté au fichier HTML.jsp. Lorsque l’utilisateur clique sur &quot;Ajouter votre photo&quot;, l’événement click de l’élément de fichier est déclenché. L’utilisateur peut ainsi parcourir et sélectionner la photo à joindre. Nous utilisons ensuite l’objet JavaScript FileReader pour obtenir la chaîne codée base64 de l’image. La chaîne d’image base64 est stockée dans le champ de texte du formulaire. Lorsque le formulaire est envoyé, nous extrayons cette valeur et l’insérons dans l’élément img du XML. Ce XML est ensuite utilisé pour fusionner avec le xdp afin de générer le pdf final.

Le profil personnalisé utilisé pour cet article vous a été mis à votre disposition dans le cadre des ressources de cet article.

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

Le code ci-dessus est exécuté lorsque nous déclenchons l’événement click de l’élément de fichier . Ligne 5 : extrait le contenu du fichier téléchargé sous la forme d’une chaîne base64 et le stocke dans le champ de texte. Cette valeur est ensuite extraite lorsque le formulaire est envoyé à notre servlet.

Nous configurons ensuite les propriétés suivantes (avancées) de notre formulaire mobile dans AEM

* Submit URL - http://localhost:4502/bin/handlemobileformsubmission. Il s’agit de notre servlet qui fusionnera les données envoyées avec le modèle xdp.
* Profil de rendu HTML : assurez-vous de sélectionner &quot;AddImageToMobileForm&quot;. Cela déclenche le code permettant d’ajouter une image au formulaire.

Pour tester cette fonctionnalité sur votre propre serveur, procédez comme suit :

* [Déploiement du lot AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Déploiement du développement avec le lot Utilisateur du service](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et installez le package associé à cet article.](assets/pdf-from-mobile-form-submission.zip)

* Assurez-vous que l’URL d’envoi et le profil de rendu HTML sont correctement définis en affichant la page des propriétés de [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Aperçu du XDP au format html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* Ajoutez une image au formulaire et envoyez-la. Vous devriez récupérer le fichier PDF avec l’image qu’il contient.

