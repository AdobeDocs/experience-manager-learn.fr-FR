---
title: Renseigner le formulaire adaptatif à l’aide de la méthode setData
description: Envoyer le fichier PDF chargé et extraire les données pour renseigner le formulaire adaptatif.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 53
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 100%

---

# Lancer un appel Ajax

Une fois le chargement du fichier PDF effectué, nous devons effectuer un appel POST à un servlet et transmettre le document PDF chargé dans la requête POST. La requête POST renvoie un chemin d’accès aux données exportées dans le référentiel CRX.

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

Le servlet monté sur **_/bin/ExtractDataFromPDF_** extrait les données du fichier PDF et renvoie le chemin d’accès au nœud CRX contenant les données extraites.
La méthode [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) est ensuite utilisée pour définir les données du formulaire adaptatif.

## Étapes suivantes

[Déployer des exemples de ressources](./test-the-solution.md)
