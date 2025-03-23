---
title: Extraire la réponse d’un envoi personnalisé
description: Extraire la réponse personnalisée lors de l’envoi réussi du formulaire
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
duration: 16
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 100%

---

# Extraire l’objet JSON de la réponse

Lorsque vous envoyez un formulaire adaptatif découplé à un gestionnaire d’envoi personnalisé, les données renvoyées par celui-ci peuvent être extraites et affichées dans votre application React. L’extrait de code suivant affiche les éléments suivants :

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
