---
title: Affichage des formulaires récupérés en mode Carte
description: Utiliser l’API listforms pour afficher les formulaires
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
exl-id: c01ad68e-23c9-4564-8e3e-1924af34a493
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 1%

---

# Récupérer et afficher les formulaires au format de carte

Le format d’affichage Carte est un modèle de conception qui présente des informations ou des données sous la forme de cartes. Chaque carte représente un élément distinct de contenu ou de saisie de données et se compose généralement d’un conteneur visuellement distinct avec des éléments spécifiques organisés dans celui-ci.
Les cartes cliquables dans React sont des composants interactifs qui ressemblent à des cartes ou des mosaïques et peuvent être cliqués ou Appuyés par l’utilisateur. Lorsqu’un utilisateur clique ou appuie sur une carte cliquable, cela déclenche une action ou un comportement spécifié, comme la navigation vers une autre page, l’ouverture d’un modal ou la mise à jour de l’interface utilisateur.

Dans cet article, nous utiliserons la variable [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) pour récupérer les formulaires et les afficher au format carte, ouvrez le formulaire adaptatif sur l’événement click.

![carte-view](./assets/card-view-forms.png)

## Modèle de carte

Le code suivant a été utilisé pour concevoir le modèle de carte. Le modèle de carte affiche le titre et la description du formulaire adaptatif, ainsi que le logo de l’Adobe. [Composants de l’interface utilisateur matérielle](https://mui.com/) ont été utilisés pour créer cette mise en page.



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
   
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.id}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

L’itinéraire suivant a été défini dans le fichier Main.js pour accéder à DisplayForm.js.

```javascript
    <Route path="/displayForm/:formID" element={<DisplayForm/>} exact/>
```

## Récupération des formulaires

L’API listforms a été utilisée pour récupérer les formulaires à partir du serveur AEM. L’API renvoie un tableau d’objets JSON, chaque objet JSON représentant un formulaire.

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

Dans le code ci-dessus, nous itérons via la fonction fetchedForms à l’aide de la fonction map et pour chaque élément du tableau fetchedForms, un composant FormCard est créé et ajouté au conteneur Grid. Vous pouvez désormais utiliser le composant ListForm dans votre application React en fonction de vos besoins.

## Étapes suivantes

[Afficher le formulaire adaptatif lorsque l’utilisateur clique sur une carte](./open-form-card-view.md)
