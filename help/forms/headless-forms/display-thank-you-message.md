---
title: Afficher le message de remerciement lors de l’envoi du formulaire
description: Utilisez le gestionnaire onSubmitSuccess pour afficher le message de remerciement configuré dans l’application de réaction.
feature: Adaptive Forms
version: 6.5
kt: 13490
topic: Development
role: User
level: Intermediate
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Afficher le message de remerciement configuré

Un message de remerciement lors de l’envoi du formulaire est un moyen réfléchi d’exprimer votre reconnaissance à l’utilisateur pour avoir rempli et envoyé un formulaire. Il confirme que leur demande a été reçue et appréciée. Le message de remerciement est configuré à l’aide de l’onglet d’envoi du conteneur du guide du formulaire adaptatif.

![merci-vous-message](assets/thank-you-message.png)

Le message de remerciement configuré est accessible dans le gestionnaire d’événements onSuccess du super composant AdaptiveForm.
Le code permettant d’associer l’événement onSuccess et le code du gestionnaire d’événements onSuccess sont répertoriés ci-dessous.

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

Le code complet du composant de fonction Contact est indiqué ci-dessous.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

Le code ci-dessus utilise des composants HTML natifs qui sont mappés aux composants utilisés dans le formulaire adaptatif. Par exemple, nous mappons le composant de formulaire adaptatif d’entrée de texte au composant TextField. Composants natifs utilisés dans l’article [peut être téléchargé ici](./assets/native-components.zip)

