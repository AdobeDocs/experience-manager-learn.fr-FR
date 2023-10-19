---
title: Renseigner le formulaire adaptatif à l’aide de la méthode setData
description: Envoyez le fichier pdf chargé pour l’extraction des données et renseignez le formulaire adaptatif avec les données extraites.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 4%

---

# Lancer un appel Ajax

Lorsque l’utilisateur a téléchargé le fichier pdf, nous devons effectuer un appel de POST à un servlet et transmettre le document de PDF téléchargé dans la demande de POST. La requête du POST renvoie un chemin d’accès aux données exportées dans le référentiel crx.

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

Le servlet monté sur **_/bin/ExtractDataFromPDF_** extrait les données du fichier du PDF et renvoie le chemin du noeud crx où les données extraites sont stockées.
La variable [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) est ensuite utilisée pour définir les données du formulaire adaptatif.

## Étapes suivantes

[Déployer des exemples de ressources](./test-the-solution.md)


