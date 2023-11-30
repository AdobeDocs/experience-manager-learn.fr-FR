---
title: Récupérer le fichier JSON du formulaire adaptatif à incorporer
description: Utiliser l’API pour récupérer le fichier JSON du formulaire adaptatif
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 100%

---

# Récupérer le fichier JSON du formulaire

Connectez-vous à votre instance de création AEM Forms et créez un formulaire adaptatif à l’aide du modèle **Vierge avec composants principaux**. Publiez le formulaire sur votre instance de publication.

Pour incorporer le formulaire, récupérez d’abord le fichier JSON du formulaire adaptatif à l’aide d’un appel GET au serveur de publication.

L’extrait de code suivant permet de récupérer le code JSON du formulaire adaptatif appelé **contactus**.

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

Le code complet du composant de la fonction Contact est indiqué ci-dessous.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

Le code ci-dessus utilise des composants HTML natifs qui sont mappés aux composants du formulaire adaptatif. Par exemple, le composant de formulaire adaptatif de saisie de texte est mappé au composant TextField. Les composants natifs décrits dans l’article [peuvent être téléchargés ici](./assets/native-components.zip).

## Étapes suivantes

[Sélectionner un formulaire dans la liste déroulante](./select-form-from-drop-down-list.md)
