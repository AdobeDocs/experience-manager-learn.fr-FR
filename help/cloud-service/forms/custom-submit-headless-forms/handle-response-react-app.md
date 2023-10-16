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
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
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
