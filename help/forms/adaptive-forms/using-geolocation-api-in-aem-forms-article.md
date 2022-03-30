---
title: Utilisation des API de géolocalisation dans Forms adaptatif
description: Renseignez les champs d’adresse de votre formulaire à l’aide des API de géolocalisation.
feature: Adaptive Forms
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 4%

---

# Utilisation des API de géolocalisation dans Forms adaptatif{#using-geolocation-api-s-in-adaptive-forms}

Dans cet article, nous allons examiner l’utilisation de l’API de géolocalisation Google pour renseigner les champs d’un formulaire adaptatif. Ce cas pratique est généralement utilisé lorsque vous souhaitez remplir les champs d’adresse actuels d’un formulaire.

Les étapes suivantes ont été suivies pour utiliser l’API de géolocalisation dans Forms adaptatif.

1. [Obtenir la clé API](https://developers.google.com/maps/documentation/javascript/get-api-key) de Google pour utiliser la plateforme Google Maps. Vous pouvez obtenir une clé d’évaluation valable pendant 1 an.

1. Un fragment de formulaire adaptatif a été créé avec des champs pour contenir l’adresse actuelle.

1. L’API de géolocalisation a été appelée sur l’événement click de l’objet image du formulaire adaptatif.

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

![Champs renseignés avec l’api geoloaction](assets/capture-4.gif)

Dans la ligne 1, nous utilisons l’API de géolocalisation de HTML pour obtenir la position actuelle. Une fois l’emplacement actuel obtenu, nous transmettons l’emplacement actuel à la fonction showPosition .

Dans la fonction showPosition , nous utilisons l’API Google pour récupérer les détails de l’adresse pour la latitude et la longitude données.

Le code JSON renvoyé par l’API est ensuite analysé pour définir les champs de formulaire adaptatif.

>[!NOTE]
>
>À des fins de test, vous pouvez utiliser le protocole HTTP avec localhost dans l’URL.
>
>Pour le serveur de production, vous devez activer SSL pour votre serveur AEM afin d’obtenir cette fonctionnalité.
>
>L’exemple associé à cet article a été testé avec l’adresse des États-Unis. Si vous souhaitez utiliser cette fonctionnalité dans d’autres emplacements géographiques, vous devrez peut-être ajuster l’analyse JSON.

Pour activer cette fonctionnalité sur votre serveur, procédez comme suit :

* Installez et démarrez le serveur AEM Forms.

>!![NOTE] Cette fonctionnalité a été testée sur AEM Forms 6.3 et versions ultérieures.
* [Obtenir la clé API Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importez dans AEM les actifs liés à cet article.](assets/geolocationapi.zip)
* [Ouvrez le fragment de formulaire adaptatif en mode d’édition.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Ouvrez l’éditeur de règles pour le composant Choix d’image .
* Remplacez la variable &lt;your_api_key> avec la clé API Google.
* Enregistrez vos modifications.
* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Cliquez sur l&#39;icône &quot;géolocalisation&quot;.
* Votre formulaire doit être renseigné avec votre emplacement actuel.
