---
title: Afficher des images DAM intégrées dans les formulaires adaptatifs
description: Afficher des images DAM intégrées dans les formulaires adaptatifs
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '210'
ht-degree: 100%

---

# Afficher une image DAM dans les formulaires adaptatifs

Un cas d’utilisation courant consiste à afficher la ou les images résidant dans le référentiel crx intégré dans un formulaire adaptatif.

## Ajouter une image d’espace réservé

La première étape consiste à ajouter une balise div d’espace réservé au composant de panneau. Dans le code ci-dessous, le composant de panneau est identifié par son nom de classe CSS, photo-upload. La fonction JavaScript fait partie de la bibliothèque cliente associée aux formulaires adaptatifs. Cette fonction est appelée dans l’événement d’initialisation du composant de pièce jointe.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Afficher une image intégrée

Une fois que l’utilisateur ou l’utilisatrice a sélectionné l’image, le champ masqué ImageName est renseigné avec le nom de l’image sélectionnée. Ce nom d’image est ensuite transmis à la fonction damURLToFile qui appelle la fonction createFile pour convertir une URL en objet Blob pour FileReader.readAsDataURL().

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

* Téléchargez et installez la [bibliothèque cliente et les exemples d’images](assets/InlineDAMImage.zip) sur votre instance AEM à l’aide du gestionnaire de packages AEM.
* Téléchargez et installez l’[exemple de formulaire](assets/FieldInspectionForm.zip) sur votre instance AEM à l’aide du gestionnaire de packages AEM.
* Pointez votre navigateur sur [FielInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled).
* Sélectionnez l’un des éléments.
* L’image doit s’afficher dans le formulaire.
