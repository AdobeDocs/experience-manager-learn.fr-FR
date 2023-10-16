---
title: Sélectionner un formulaire dans une liste de formulaires disponibles
description: Utilisation de l’API listforms pour remplir la liste déroulante
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 4%

---

# Sélectionner un formulaire à remplir dans une liste déroulante

Les listes déroulantes offrent un moyen compact et organisé de présenter une liste d’options aux utilisateurs. Les éléments de la liste déroulante seront renseignés avec les résultats de [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![carte-view](./assets/forms-drop-down.png)

## Liste déroulante

Le code suivant a été utilisé pour remplir la liste déroulante avec les résultats de l’appel de l’API listforms. Selon la sélection de l’utilisateur, le formulaire adaptatif s’affiche pour que l’utilisateur puisse le remplir et l’envoyer. [Composants de l’interface utilisateur matérielle](https://mui.com/) ont été utilisés lors de la création de cette interface ;

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

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
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
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
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

* Formulaire de récupération : un appel get est effectué au [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition), où l’identifiant est l’identifiant du formulaire adaptatif sélectionné par l’utilisateur dans la liste déroulante. Le résultat de cet appel de GET est stocké dans selectedForm.

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* Afficher le formulaire sélectionné. Le code suivant a été utilisé pour afficher le formulaire sélectionné. L’élément AdaptiveForm est fourni dans le package npm aemforms/af-response-renderer et il attend les mappages et le formJson comme ses propriétés.

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Étapes suivantes

[Afficher les formulaires en mise en page Carte](./display-forms-card-view.md)
