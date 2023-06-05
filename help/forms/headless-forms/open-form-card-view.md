---
title: Cliquez sur la carte pour afficher le formulaire.
description: Exploration du formulaire à partir du mode Carte
feature: Adaptive Forms
version: 6.5
kt: 13372
topic: Development
role: User
level: Intermediate
source-git-commit: 3bbf80d5c301953b3a34ef8256702ac7445c40da
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---

# Affichage du formulaire en cas de clic sur la carte

Le code suivant a été utilisé pour afficher le formulaire lorsque l’utilisateur clique sur une carte. Le chemin d’accès du formulaire à afficher est extrait de l’URL à l’aide de la fonction useParams .

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
   const pathParam = params["*"];
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    // Add the leading '/' back on 
   const path = '/' + pathParam;
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`${path}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON)
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

Dans le code ci-dessus, le chemin d’accès du formulaire à générer est extrait de l’URL et utilisé dans l’appel de récupération pour récupérer le fichier JSON du formulaire. Le code JSON récupéré se trouve alors dans le code suivant :

```javascript
        <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
```
