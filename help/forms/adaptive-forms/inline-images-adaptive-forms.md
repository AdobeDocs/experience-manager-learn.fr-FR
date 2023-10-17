---
title: Afficher des images intégrées dans des formulaires adaptatifs
description: Afficher des images chargées intégrées dans des formulaires adaptatifs
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '225'
ht-degree: 100%

---

# Images intégrées dans les formulaires adaptatifs

Il est arrive souvent d’afficher l’image chargée en tant qu’image intégrée dans un formulaire adaptatif. Par défaut, l’image chargée s’affiche sous forme de lien et cette expérience peut être améliorée en affichant l’image dans le formulaire adaptatif. Cet article décrit les étapes à suivre pour afficher une image intégrée.

## Ajouter une image d’espace réservé

La première étape consiste à ajouter une balise div d’espace réservé au composant de pièce jointe. Dans le code ci-dessous, le composant de pièce jointe est identifié par son nom de classe CSS, photo-upload. La fonction JavaScript fait partie de la bibliothèque cliente associée aux formulaires adaptatifs. Cette fonction est appelée dans l’événement d’initialisation du composant de pièce jointe.

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

Une fois que l’utilisateur ou l’utilisatrice a chargé l’image, la fonction répertoriée ci-dessous est appelée dans l’événement d’engagement du composant de pièce jointe. La fonction reçoit l’objet de fichier chargé en tant qu’argument.

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

* Téléchargez et installez la [bibliothèque cliente](assets/inline-image-client-library.zip) sur votre instance AEM à l’aide du gestionnaire de packages AEM.
* Téléchargez et installez l’[exemple de formulaire](assets/inline-image-af.zip) sur votre instance AEM à l’aide du gestionnaire de packages AEM.
* Pointez votre navigateur sur [Ajouter une image intégrée](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled).
* Cliquez sur le bouton « Joindre votre photo » pour ajouter une image.
