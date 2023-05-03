---
title: Incorporation de formulaires Forms/HTML5 adaptatifs dans une page web
description: Étapes de configuration nécessaires pour incorporer des formulaires Forms adaptatifs ou HTML5 dans une page web non AEM.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 13%

---

# Incorporation d’un formulaire adaptatif ou d’un HTML 5 dans une page web

Le formulaire adaptatif incorporé est entièrement fonctionnel et les utilisateurs et utilisatrices peuvent le remplir et l’envoyer sans quitter la page. Il permet à l’utilisateur de rester dans le contexte d’autres éléments de la page web et d’interagir simultanément avec le formulaire.

La vidéo suivante explique les étapes nécessaires à l’intégration d’un formulaire adaptatif ou HTML5 dans une page web.
Reportez-vous à la section [documentation](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html) pour connaître les bonnes conditions préalables, les bonnes pratiques, etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

Vous pouvez télécharger les exemples de fichiers utilisés dans la vidéo. [ici](assets/embedding-af-web-page.zip)

Voici le code utilisé pour récupérer le formulaire adaptatif et incorporer le formulaire dans le conteneur identifié par le nom de classe. **right**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
