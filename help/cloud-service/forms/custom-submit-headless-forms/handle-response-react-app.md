---
title: Extraction de la réponse d’un envoi personnalisé
description: Extraire la réponse personnalisée lors de l’envoi réussi du formulaire
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---


# Extraire l’objet json de la réponse

Lorsque vous envoyez un formulaire adaptatif sans en-tête à un gestionnaire d’envoi personnalisé, les données renvoyées par le gestionnaire d’envoi peuvent être extraites et affichées dans votre application de réaction. Le fragment de code suivant affiche

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```











