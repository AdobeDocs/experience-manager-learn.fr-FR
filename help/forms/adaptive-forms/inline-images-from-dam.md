---
title: Affichage d’images intégrées dans Adaptive Forms
description: Affichage des images téléchargées en ligne dans Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
kt: kt-11307
source-git-commit: 853c4fedd4b8db594aa0b53fd2d27d996811f14e
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Affichage de l’image DAM dans Forms adaptatif

Un cas d’utilisation courant consiste à afficher la ou les images résidant dans le référentiel crx intégré dans un formulaire adaptatif.

## Ajout d’une image d’espace réservé

La première étape consiste à ajouter une balise &lt;div> d’espace réservé au composant de panneau. Dans le code ci-dessous, le composant de panneau est identifié par son nom de classe CSS de chargement de photo. La fonction JavaScript fait partie de la bibliothèque cliente associée aux formulaires adaptatifs. Cette fonction est appelée dans l’événement initialize du composant de pièce jointe.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Afficher une image intégrée

Une fois que l’utilisateur a sélectionné l’image, le champ masqué ImageName est renseigné avec le nom de l’image sélectionné. Ce nom d’image est ensuite transmis à la fonction damURLToFile qui appelle la fonction createFile pour convertir une URL en objet Blob pour FileReader.readAsDataURL().

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### Déployer sur votre serveur

* Téléchargez et installez le [bibliothèque cliente et exemples d’images](assets/InlineDAMImage.zip) sur votre instance AEM à l’aide d’AEM Package Manager.
* Téléchargez et installez le [exemple de formulaire](assets/FieldInspectionForm.zip) sur votre instance AEM à l’aide d’AEM gestionnaire de packages.
* Pointez votre navigateur sur [FieldInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Sélectionner l’un des accessoires
* L’image doit s’afficher dans le formulaire.