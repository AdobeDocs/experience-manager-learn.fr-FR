---
title: Incorporation de formulaires Forms/HTML5 adaptatifs dans une page web
description: Étapes de configuration nécessaires pour incorporer des formulaires Forms adaptatifs ou HTML5 dans une page web non AEM.
feature: Formulaires adaptatifs
type: Tutorial
version: 6.4,6.5
topic: Développement
role: Developer
level: Beginner
kt: 8390
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 28%

---


# Incorporation d’un formulaire adaptatif ou d’un formulaire HTML5 dans une page web

Le formulaire adaptatif incorporé est entièrement fonctionnel et les utilisateurs peuvent le remplir et le soumettre sans quitter la page. Il permet à l’utilisateur de rester dans le contexte des autres éléments de la page Web et d’interagir simultanément avec le formulaire.

La vidéo suivante explique les étapes nécessaires à l’intégration d’un formulaire adaptatif ou HTML5 dans une page web.
Reportez-vous à la [documentation](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) pour connaître les bonnes conditions préalables, les bonnes pratiques, etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

Vous pouvez télécharger les exemples de fichiers utilisés dans la vidéo [à partir d’ici](assets/embedding-af-web-page.zip)

Voici le code utilisé pour récupérer le formulaire adaptatif et incorporer le formulaire dans le conteneur identifié par le nom de classe **right**

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```














