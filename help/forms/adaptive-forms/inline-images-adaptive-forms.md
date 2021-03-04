---
title: Affichage d’images intégrées dans une Forms adaptative
seo-title: Affichage d’images intégrées dans une Forms adaptative
description: Affichage des images téléchargées en ligne dans Forms adaptatif
seo-description: Affichage des images téléchargées en ligne dans Forms adaptatif
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---


# Images intégrées dans Adaptive Forms

Un cas d’utilisation courant consiste à afficher l’image téléchargée en tant qu’image intégrée dans un formulaire adaptatif. Par défaut, l’image téléchargée s’affiche sous forme de lien et cette expérience peut être améliorée en affichant l’image dans un formulaire adaptatif. Cet article décrit les étapes d&#39;affichage des images intégrées.

## Image d’espace réservé Ajouté

La première étape consiste à ajouter en préfixe une balise d&#39;emplacement div au composant de pièce jointe du fichier. Dans le code ci-dessous, le composant de pièce jointe est identifié par son nom de classe CSS de téléchargement de photos. La fonction JavaScript fait partie de la bibliothèque cliente associée aux formulaires adaptatifs. Cette fonction est appelée dans l’initialisation du événement du composant de pièce jointe du fichier.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Afficher l’image intégrée

Une fois que l’utilisateur a téléchargé l’image, la fonction répertoriée ci-dessous est appelée dans le événement de validation du composant de pièce jointe du fichier. La fonction reçoit l’objet de fichier téléchargé en tant qu’argument.

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

* Téléchargez et installez la [bibliothèque cliente](assets/inline-image-client-library.zip) sur votre instance AEM à l’aide d’AEM Package Manager.
* Téléchargez et installez l&#39;[exemple de formulaire](assets/inline-image-af.zip) sur votre instance AEM à l&#39;aide d&#39;AEM Package Manager.
* Pointez votre navigateur sur [Ajouter l’image intégrée](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled).
* Cliquez sur le bouton &quot;Joindre votre photo&quot; pour ajouter une image.