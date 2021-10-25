---
title: Quelques conseils et astuces utiles sur l’interface utilisateur
description: Document pour montrer quelques conseils utiles sur l’interface utilisateur
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 280ea1ec8fc5da644320753958361488872359cc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 8%

---

# Activation/désactivation de la visibilité du champ de mot de passe

Un cas pratique courant consiste à permettre à l’utilisateur de basculer vers la visibilité du texte saisi dans le champ du mot de passe.
Pour réaliser ce cas d’utilisation, j’ai utilisé l’icône en forme d’oeil du [Bibliothèque Font Awesome](https://fontawesome.com/). Les fichiers CSS requis et eye.svg sont inclus dans la bibliothèque cliente créée pour cette démonstration.

## Exemple en direct

[Cette fonctionnalité peut être testée ici](https://forms.enablementadobe.com/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)

## Exemple de code

Le formulaire adaptatif comporte un champ de type PasswordBox appelé **ssnField**.

Le code suivant est exécuté au chargement du formulaire.

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

Le CSS suivant a été utilisé pour positionner la variable **oeil** dans le champ du mot de passe

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## Déploiement de l’exemple de mot de passe de basculement

* Téléchargez la [bibliothèque cliente](assets/simple-ui-tips.zip)
* Téléchargez la [exemple de formulaire](assets/simple-ui-tricks-form.zip)
* Importez la bibliothèque cliente à l’aide de la méthode [interface utilisateur du gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Importez l’exemple de formulaire à l’aide du [Forms et document](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


