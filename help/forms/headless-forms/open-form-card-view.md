---
title: Cliquez sur la carte pour afficher le formulaire.
description: Analyser en profondeur un formulaire dans la vue Carte
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13372
topic: Development
role: User
level: Intermediate
exl-id: c8684cd9-b9c5-4b5b-b990-27c5700cea9f
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '61'
ht-degree: 100%

---

# Afficher un formulaire en cliquant sur la carte

Le code suivant a été utilisé pour afficher le formulaire lorsque l’utilisateur ou l’utilisatrice clique sur une carte. Le chemin d’accès du formulaire à afficher est extrait de l’URL à l’aide de la fonction useParams.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`/adobe/forms/af/${params.formID}`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```

## Étapes suivantes

[Afficher le message de remerciement lors de l’envoi du formulaire](./display-thank-you-message.md)
