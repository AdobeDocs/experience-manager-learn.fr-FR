---
title: Fonctionnalité de saisie automatique dans AEM Forms
description: Permet aux utilisateurs de rechercher et de sélectionner rapidement dans une liste préremplie de valeurs au fur et à mesure de leur saisie, en exploitant la recherche et le filtrage.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 4af14b7d72ebdbea04e68a9a64afa1a96d1c1aeb
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# Mise en oeuvre automatique

Mettez en oeuvre la fonctionnalité de saisie automatique dans AEM forms à l’aide de la fonction de saisie automatique de jquery.
L’exemple inclus dans cet article utilise diverses sources de données (tableau statique, tableau dynamique renseigné à partir d’une réponse de l’API REST) pour renseigner les suggestions lorsque l’utilisateur commence à saisir du texte dans le champ.

Le code utilisé pour accomplir la fonctionnalité de saisie semi-automatique est associé à l’événement initialize du champ.

## Suggestion d’adresse

![suggestions de pays](assets/auto-complete2.png)



Voici le code utilisé pour fournir des suggestions d’adresses de rue :

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```





## Suggestions avec l’émoticône

![suggestions de pays](assets/auto-complete3.png)

Le code suivant a été utilisé pour afficher les émoticônes dans la liste de suggestions.

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

Le [exemple de formulaire peut être téléchargé](assets/auto-complete-form.zip) d’ici. Veillez à fournir votre propre nom d’utilisateur/clé d’API à l’aide de l’éditeur de code pour que le code effectue des appels REST réussis.

>[!NOTE]
>
> Pour que la saisie automatique fonctionne, assurez-vous que votre formulaire utilise la bibliothèque cliente suivante : **cq.jquery.ui**. Cette bibliothèque cliente est fournie avec AEM.
