---
title: Création de bibliothèques clientes
description: Créez une bibliothèque cliente pour gérer le événement de clics du bouton "Enregistrer et quitter".
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 6%

---

# Créer une bibliothèque cliente

Créez [client lib](https://docs.adobe.com/content/help/fr-FR/experience-manager-65/developing/introduction/clientlibs.html) qui inclura le code pour appeler la méthode `doAjaxSubmitWithFileAttachment` de l&#39;API `guideBridge` sur le événement de clic du bouton identifié par la classe CSS **savebutton**.  Nous transmettons les données du formulaire adaptatif, `fileMap`, et `mobileNumber` au point de terminaison en écoutant `**/bin/storeafdatawithattachments`

Une fois les données du formulaire enregistrées, un identifiant d’application unique est généré et présenté à l’utilisateur dans une boîte de dialogue. Lors de la fermeture de la boîte de dialogue, l’utilisateur est amené au formulaire qui lui permet de récupérer le formulaire adaptatif enregistré à l’aide de l’identifiant d’application unique.

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
> Nous avons utilisé [bootbox javascript library](http://bootboxjs.com/examples.html) pour afficher la boîte de dialogue.

Les bibliothèques clientes utilisées dans cet exemple peuvent être [téléchargées ici](assets/client-libraries.zip)
