---
title: Utiliser les API Geolocation dans les formulaires adaptatifs
description: Renseignez les champs d’adresse de votre formulaire à l’aide des API Geolocation.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '390'
ht-degree: 100%

---

# Utiliser les API Geolocation dans les formulaires adaptatifs{#using-geolocation-api-s-in-adaptive-forms}

Dans cet article, nous allons utiliser l’API Geolocation de Google pour renseigner les champs d’un formulaire adaptatif. Ce cas d’utilisation s’adresse aux personnes qui souhaitent remplir les champs de l’adresse actuelle d’un formulaire.

Pour utiliser l’API Geolocation dans les formulaires adaptatifs, procédez comme suit.

1. [Obtenez la clé d’API](https://developers.google.com/maps/documentation/javascript/get-api-key) auprès de Google pour utiliser la plateforme Google Maps. Vous pouvez obtenir une clé d’évaluation, valable 1 an.

1. Un fragment de formulaire adaptatif a été créé et comporte des champs relatifs à l’adresse actuelle.

1. L’API Geolocation a été appelée lors de l’événement de clic sur l’objet image du formulaire adaptatif.

1. Les données JSON renvoyées par l’appel API ont été analysées et les valeurs des champs de formulaire adaptatif ont été définies en conséquence.

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![Champs renseignés avec l’API Geolocation](assets/capture-4.gif).

Dans la ligne 1, l’API Geolocation HTML permet d’obtenir la position actuelle. Une fois l’emplacement actuel obtenu, l’emplacement actuel est transmis à la fonction showPosition.

Dans la fonction showPosition, l’API Google permet de récupérer les détails de l’adresse pour la latitude et la longitude données.

Le code JSON renvoyé par l’API est ensuite analysé pour définir les champs de formulaire adaptatif.

>[!NOTE]
>
>À des fins de test, vous pouvez utiliser le protocole HTTP avec localhost dans l’URL.
>
>Pour le serveur de production, vous devez activer le protocole SSL pour votre serveur AEM afin de bénéficier de cette fonctionnalité.
>
>L’exemple présenté dans cet article concerne une adresse aux États-Unis. Si vous souhaitez utiliser cette fonctionnalité dans d’autres pays, vous devrez peut-être ajuster l’analyse JSON.

Pour activer cette fonctionnalité sur votre serveur, procédez comme suit :

* Installez et démarrez le serveur AEM Forms.
> Cette fonctionnalité a été testée sur AEM Forms 6.3 et versions ultérieures.
* [Obtenez la clé d’API Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importez dans AEM les ressources présentées dans cet article.](assets/geolocationapi.zip)
* [Ouvrez le fragment de formulaire adaptatif en mode d’édition.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Ouvrez l’éditeur de règles pour le composant Choix d’image.
* Remplacez &lt;your_api_key> par la clé d’API Google.
* Enregistrez vos modifications.
* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Cliquez sur l’icône « geolocation ».
* Votre emplacement actuel est à présent renseigné dans le formulaire.
