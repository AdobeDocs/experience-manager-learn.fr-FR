---
title: Sélectionner un formulaire dans une liste des formulaires disponibles
description: Utiliser l’API listforms pour remplir la liste déroulante
feature: Adaptive Forms
version: 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 100%

---

# Sélectionner un formulaire à remplir dans une liste déroulante

Les listes déroulantes peuvent être utilisées pour présenter différentes options aux utilisateurs et utilisatrices, dans un format compact et concis. Les éléments de la liste déroulante correspondent aux résultats de l’[API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms).

![card-view](./assets/forms-drop-down.png)

## Liste déroulante

Le code suivant permet de renseigner la liste déroulante avec les résultats de l’appel d’API listforms. Suivant la sélection de l’utilisateur ou de l’utilisatrice, le formulaire adaptatif pertinent lui est présenté et peut être complété et envoyé. Des [Composants Material UI](https://mui.com/) ont été utilisés pour créer cette interface.

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

Les deux appels API suivants permettent de créer cette interface utilisateur.

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). L’appel de récupération des formulaires n’est effectué qu’une seule fois, au moment du rendu du composant. Les résultats de l’appel API sont stockés dans la variable afForms.
Dans le code ci-dessus, nous itérons à travers afForms à l’aide de la fonction map et pour chaque élément du tableau afForms, un composant MenuItem est créé et ajouté au composant Select.

* Récupérer le formulaire : un appel GET est effectué à [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition), dans lequel l’identifiant correspond à l’identifiant du formulaire adaptatif sélectionné par l’utilisateur ou l’utilisatrice dans la liste déroulante. Le résultat de cet appel GET est stocké dans selectedForm.

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* Affichez le formulaire sélectionné. Le code suivant permet d’afficher le formulaire sélectionné. L’élément AdaptiveForm est fourni dans le package npm aemforms/af-response-renderer et attend les mappages et le formJson comme propriétés.

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Étapes suivantes

[Afficher les formulaires dans la disposition Carte](./display-forms-card-view.md)
