---
title: Affichage d’images intégrées dans Adaptive Forms
description: Affichage des images téléchargées en ligne dans Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# Images intégrées dans Adaptive Forms

Un cas d’utilisation courant consiste à afficher l’image téléchargée en tant qu’image intégrée dans un formulaire adaptatif. Par défaut, l’image téléchargée s’affiche sous forme de lien et cette expérience peut être améliorée en affichant l’image dans le formulaire adaptatif. Cet article décrit les étapes à suivre pour afficher une image intégrée.

## Ajout d’une image d’espace réservé

La première étape consiste à ajouter une balise div d’espace réservé au composant de pièce jointe. Dans le code ci-dessous, le composant de pièce jointe est identifié par son nom de classe CSS de téléchargement de photo. La fonction JavaScript fait partie de la bibliothèque cliente associée aux formulaires adaptatifs. Cette fonction est appelée dans l’événement initialize du composant de pièce jointe.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Afficher une image intégrée

Une fois que l’utilisateur a chargé l’image, la fonction répertoriée ci-dessous est appelée dans l’événement commit du composant de pièce jointe. La fonction reçoit l’objet de fichier chargé en tant qu’argument.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### Déployer sur votre serveur

* Téléchargez et installez le [bibliothèque cliente](assets/inline-image-client-library.zip) sur votre instance AEM à l’aide d’AEM gestionnaire de packages.
* Téléchargez et installez le [exemple de formulaire](assets/inline-image-af.zip) sur votre instance AEM à l’aide d’AEM gestionnaire de packages.
* Pointez votre navigateur sur [Ajout d’une image intégrée](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Cliquez sur le bouton &quot;Joindre votre photo&quot; pour ajouter une image.
