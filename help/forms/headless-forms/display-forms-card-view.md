---
title: Affichage des formulaires récupérés en mode Carte
description: Utiliser l’API listforms pour afficher les formulaires
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Récupérer et afficher les formulaires au format de carte

Le format d’affichage Carte est un modèle de conception qui présente des informations ou des données sous la forme de cartes. Chaque carte représente un élément distinct de contenu ou de saisie de données et se compose généralement d’un conteneur visuellement distinct avec des éléments spécifiques organisés dans celui-ci. Dans cet article, nous utiliserons la variable [API listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) pour récupérer les formulaires et les afficher au format de carte, comme illustré ci-dessous

![carte-view](./assets/card-view-forms.png)

## Modèle de carte

Le code suivant a été utilisé pour concevoir le modèle de carte. Le modèle de carte affiche le titre et la description du formulaire adaptatif, ainsi que le logo de l’Adobe. [Composants de l’interface utilisateur matérielle](https://mui.com/) ont été utilisés pour créer cette mise en page.

```javascript
import Paper from "@mui/material/Paper";
import Grid from "@mui/material/Grid";
import Container from "@mui/material/Container";
import { Typography } from "@mui/material";
import { Box } from "@mui/system";
const FormCard =({headlessForm}) => {
    return (
              <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                    <Typography variant="subtititle2" component="h2">
                        {headlessForm.title}
                    
                    </Typography>
                    <Typography variant="subtititle3" component="h4">
                        {headlessForm.description}
                    
                    </Typography>
                    </Box>
                </Paper>
                </Grid>
          


    );
    

};
export default FormCard;
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

Dans le code ci-dessus, nous itérons via la fonction fetchedForms à l’aide de la fonction map et pour chaque élément du tableau fetchedForms, un composant FormCard est créé et ajouté au conteneur Grid. Vous pouvez désormais utiliser le composant ListForm dans votre application React selon vos besoins.
