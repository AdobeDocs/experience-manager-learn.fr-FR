---
title: Récupération du JSON du formulaire adaptatif à incorporer
description: Utilisation de l’API pour récupérer le fichier json du formulaire adaptatif
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---


# Récupération du JSON du formulaire

Connectez-vous à votre instance d’auteur AEM Forms et créez un fichier adaptatif à l’aide du **Vierge avec composants principaux** modèle. Publiez votre formulaire sur votre instance de publication.

Pour incorporer le formulaire, nous récupérons d’abord le fichier json du formulaire adaptatif en effectuant un appel get sur notre serveur de publication.

Le fragment de code suivant récupère le code json du formulaire adaptatif appelé **contactus**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
      }
```

Le code complet du composant de fonction Contact est indiqué ci-dessous.

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
        
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
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

Le code ci-dessus utilise des composants HTML natifs qui sont mappés aux composants utilisés dans le formulaire adaptatif. Par exemple, nous mappons le composant de formulaire adaptatif d’entrée de texte au composant TextField. Composants natifs utilisés dans l’article [peut être téléchargé ici](./assets/native-components.zip)

## Étapes suivantes

[Sélectionner un formulaire dans la liste déroulante](./select-form-from-drop-down-list.md)