---
title: Créer une bibliothèque cliente
description: Code de bibliothèque cliente pour récupérer le formulaire suivant à signer.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 34
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '91'
ht-degree: 100%

---

# Créer une bibliothèque cliente

Créez une bibliothèque cliente personnalisée, clientlib en abrégé, pour extraire les paramètres d’URL et transmettre ces paramètres dans l’appel GET. L’appel GET est effectué vers un servlet monté sur /bin/getnextformtosign qui renvoie l’URL du formulaire suivant pour se connecter au package.

Voici le code utilisé dans la fonction JavaScript de la clientlib.


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Ressources

[La clientlib peut être téléchargée ici.](assets/get-next-form-client-lib.zip)

## Étapes suivantes

[Créer un modèle de formulaire personnalisé pour ce cas d’utilisation](./create-af-template.md)