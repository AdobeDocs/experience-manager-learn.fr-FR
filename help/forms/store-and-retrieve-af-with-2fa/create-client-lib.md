---
title: Créer des bibliothèques clientes
description: Créez une bibliothèque cliente pour gérer l’événement de clic du bouton « Enregistrer et quitter ».
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 100%

---

# Créer une bibliothèque cliente

Créez une [bibliothèque cliente](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=fr) comprenant le code pour appeler la méthode `doAjaxSubmitWithFileAttachment` de l’API `guideBridge` lors de l’événement de clic du bouton identifié par la classe CSS **Bouton Enregistrer**.  Les données du formulaire adaptatif, `fileMap`, et la propriété `mobileNumber` sont transmises au point d’entrée en écoute à l’emplacement `**/bin/storeafdatawithattachments`.

Une fois les données du formulaire enregistrées, un identifiant d’application unique est généré et présenté à l’utilisateur ou à l’utilisatrice dans une boîte de dialogue. En fermant la boîte de dialogue, la personne utilisatrice est amenée au formulaire, ce qui lui permet de récupérer le formulaire adaptatif enregistré à l’aide de l’ID d’application unique.

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
                  x.data.applicationID +
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
> Pour afficher la boîte de dialogue, la [bibliothèque JavaScript bootbox](https://bootboxjs.com/examples.html) a été utilisée.

Les bibliothèques clientes utilisées dans cet exemple peuvent être [téléchargées ici](assets/store-af-with-attachments-client-lib.zip).

## Étapes suivantes

[Vérifier les utilisateurs et utilisatrices avec le service OTP](./verify-users-with-otp.md)
