---
title: Générer un fichier PDF à partir de l’envoi de formulaire HTML5
seo-title: Générer un fichier PDF à partir de l’envoi de formulaire HTML5
description: Générer un fichier PDF à partir de l’envoi de formulaires pour périphériques mobiles
seo-description: Générer un fichier PDF à partir de l’envoi de formulaires pour périphériques mobiles
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Générer un fichier PDF à partir de l’envoi de formulaire HTML5 {#generate-pdf-from-htm-form-submission}

Cet article décrit les étapes nécessaires à la génération de pdf à partir d’un envoi de formulaire HTML5(ou Forms mobile). Cette démonstration explique également les étapes nécessaires pour ajouter une image au formulaire HTML5 et la fusionner dans le PDF final.

Pour voir une démonstration en direct de cette fonctionnalité, consultez l’ [exemple de serveur](https://forms.enablementadobe.com/content/samples/samples.html?query=0) et recherchez &quot;Mobile Form To PDF&quot;.

Pour fusionner les données envoyées dans le modèle xdp, procédez comme suit :

Ecrire une servlet pour gérer l’envoi de formulaire HTML5

* Dans cette servlet, récupérez les données envoyées.
* Fusionner ces données avec le modèle xdp pour générer un fichier pdf
* Rendre le fichier pdf à l’application appelante

Voici le code de servlet qui extrait les données envoyées de la demande. Il appelle ensuite la méthode personnalisée documentServices .mobileFormToPDF pour obtenir le pdf.

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

Pour ajouter une image au formulaire mobile et l’afficher dans le fichier pdf, nous avons utilisé les éléments suivants :

Modèle XDP - Dans le modèle xdp, nous avons ajouté un champ d’image et un bouton appelé btnAddImage. Le code suivant traite le événement de clics de btnAddImage dans notre profil personnalisé. Comme vous pouvez le voir, nous déclenchons le événement de clics fichier1. Aucun codage n&#39;est nécessaire dans le fichier xdp pour réaliser ce cas d&#39;utilisation

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[Profil](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)personnalisé. L’utilisation d’un profil personnalisé facilite la manipulation des objets DOM HTML du formulaire pour périphériques mobiles. Un élément de fichier masqué est ajouté au fichier HTML.jsp. Lorsque l’utilisateur clique sur &quot;Ajouter votre photo&quot;, nous déclenchons le événement de clic de l’élément de fichier. Cela permet à l&#39;utilisateur de parcourir et de sélectionner la photo à joindre. Nous utilisons ensuite l’objet javascript FileReader pour obtenir la chaîne codée en base64 de l’image. La chaîne d’image base64 est stockée dans le champ de texte du formulaire. Lorsque le formulaire est envoyé, nous extrayons cette valeur et l’insérons dans l’élément img du XML. Ce code XML est ensuite utilisé pour fusionner avec xdp afin de générer le fichier pdf final.

Le profil personnalisé utilisé pour cet article vous a été rendu disponible dans le cadre des ressources de cet article.

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

Le code ci-dessus est exécuté lorsque nous déclenchons le événement de clic de l’élément de fichier. La ligne 5 extrait le contenu du fichier téléchargé sous la forme d&#39;une chaîne base64 et le stocke dans le champ de texte. Cette valeur est ensuite extraite lorsque le formulaire est envoyé à notre servlet.

Nous configurons ensuite les propriétés suivantes (avancées) de notre formulaire mobile en AEM

* URL d’envoi : http://localhost:4502/bin/handlemobileformsubmission. Il s’agit de notre servlet qui fusionnera les données envoyées avec le modèle xdp.
* PROFIL de rendu HTML : assurez-vous de sélectionner &quot;AddImageToMobileForm&quot;. Cela déclenchera le code pour ajouter une image au formulaire.

Pour tester cette fonctionnalité sur votre propre serveur, procédez comme suit :

* [Déploiement du lot AemFormsDocumentServices](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Déployer le groupe Développer avec l’utilisateur du service](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et installez le package associé à cet article.](assets/pdf-from-mobile-form-submission.zip)

* Assurez-vous que l’URL d’envoi et le profil de rendu HTML sont définis correctement en affichant la page de propriétés de [xdp.](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [Prévisualisation du fichier XDP au format html](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* ajoutez une image dans le formulaire et envoyez-la. Vous devez récupérer le fichier PDF avec l’image qu’il contient.

