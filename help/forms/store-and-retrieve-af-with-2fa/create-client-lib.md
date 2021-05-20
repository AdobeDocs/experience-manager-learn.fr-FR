---
title: Création de bibliothèques clientes
description: Créer une bibliothèque cliente pour gérer l’événement de clic du bouton "Enregistrer et quitter"
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 8%

---

# Créer une bibliothèque cliente

Créez la [bibliothèque cliente](https://docs.adobe.com/content/help/fr-FR/experience-manager-65/developing/introduction/clientlibs.html) qui inclura le code permettant d’appeler la méthode `doAjaxSubmitWithFileAttachment` de l’API `guideBridge` sur l’événement click du bouton identifié par la classe CSS **savebutton**.  Nous transmettons les données du formulaire adaptatif, `fileMap`, et `mobileNumber` au point de terminaison en écoutant `**/bin/storeafdatawithattachments`

Une fois les données du formulaire enregistrées, un identifiant d’application unique est généré et présenté à l’utilisateur dans une boîte de dialogue. Lorsque la boîte de dialogue est désactivée, l’utilisateur est amené au formulaire, ce qui lui permet de récupérer le formulaire adaptatif enregistré à l’aide de l’ID d’application unique.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> Nous avons utilisé [la bibliothèque JavaScript de bootbox](http://bootboxjs.com/examples.html) pour afficher la boîte de dialogue.

Les bibliothèques clientes utilisées dans cet exemple peuvent être [téléchargées ici](assets/client-libraries.zip)
