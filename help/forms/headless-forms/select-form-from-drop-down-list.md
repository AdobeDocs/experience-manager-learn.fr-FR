---
title: Sélectionner un formulaire dans une liste de formulaires disponibles
description: Utilisation de l’API listforms pour remplir la liste déroulante
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Sélectionner un formulaire à remplir dans une liste déroulante

Les listes déroulantes offrent un moyen compact et organisé de présenter une liste d’options aux utilisateurs. Les éléments de la liste déroulante seront renseignés avec les résultats de [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![carte-view](./assets/forms-drop-down.png)

## Liste déroulante

Le code suivant a été utilisé pour remplir la liste déroulante avec les résultats de l’appel de l’API listforms. Selon la sélection de l’utilisateur, le formulaire adaptatif s’affiche pour que l’utilisateur puisse le remplir et l’envoyer. [Composants de l’interface utilisateur matérielle](https://mui.com/) ont été utilisés pour créer cette interface.

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formPath, setFormPath] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The path is "+event.target.value) 
    
        setFormPath(event.target.value)
        console.log("The formPath"+ formPath);
     };
const getForm = async () =>
     {
        const resp = await fetch(`${formPath}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formPath]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formPath}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.path}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

Les deux appels API suivants ont été utilisés pour créer cette interface utilisateur.

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). L’appel de récupération des formulaires n’est effectué qu’une seule fois lorsque le composant est rendu. Les résultats de l’appel API sont stockés dans la variable afForms .
Dans le code ci-dessus, nous itérons à travers afForms à l’aide de la fonction map et pour chaque élément du tableau afForms, un composant MenuItem est créé et ajouté au composant Select.

* Récupérer le formulaire : un appel get est effectué au point de terminaison suivant, où formPath est le chemin d’accès au formulaire adaptatif sélectionné par l’utilisateur dans la liste déroulante. Le résultat de cet appel de GET est stocké dans selectedForm.

```
${formPath}/jcr:content/guideContainer.model.json`
```

* Afficher le formulaire sélectionné. Le code suivant a été utilisé pour afficher le formulaire sélectionné. L’élément AdaptiveForm est fourni dans le package npm aemforms/af-response-renderer et il attend les mappages et le formJson comme ses propriétés.

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Étapes suivantes

[Afficher les formulaires en mise en page Carte](./display-forms-card-view.md)



