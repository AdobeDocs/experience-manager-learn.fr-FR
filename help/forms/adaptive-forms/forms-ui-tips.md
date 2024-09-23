---
title: Conseils et astuces utiles sur l’interface utilisateur
description: Document contenant quelques conseils utiles sur l’interface utilisateur
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
last-substantial-update: 2019-06-09T00:00:00Z
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '147'
ht-degree: 100%

---

# Activer/désactiver la visibilité du champ de mot de passe

Un cas pratique courant consiste à permettre aux utilisateurs et utilisatrices de formulaire de basculer vers la visibilité du texte saisi dans le champ du mot de passe.
Pour réaliser ce cas d’utilisation, j’ai utilisé l’icône en forme d’œil de la [Bibliothèque de polices Awesome](https://fontawesome.com/). Les fichiers CSS requis et eye.svg sont inclus dans la bibliothèque cliente créée pour cette démonstration.


## Exemple de code

Le formulaire adaptatif comporte un champ de type PasswordBox appelé **ssnField**.

Le code suivant est exécuté lors du chargement du formulaire.

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

Le CSS suivant a été utilisé pour positionner l’icône **œil** dans le champ du mot de passe.

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

## Déployez l’exemple de bouton (bascule) du mot de passe.

* Téléchargez la [bibliothèque cliente](assets/simple-ui-tips.zip).
* Téléchargez l’[exemple de formulaire](assets/simple-ui-tricks-form.zip).
* Importez la bibliothèque cliente à l’aide de l’[interface utilisateur du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Importez l’exemple de formulaire à l’aide de [Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled).


