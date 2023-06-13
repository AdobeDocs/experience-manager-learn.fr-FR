---
title: Ajout d’un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi (RTE)
description: Découvrez comment ajouter un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi (RTE) dans l’éditeur de fragment de contenu AEM
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
source-git-commit: c54d078c6282f8ace936dd4a9ee0d5cc39490230
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# Ajout d’un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi (RTE)

![Exemple d’extension de barre d’outils de l’éditeur de fragments de contenu](./assets/rte-toolbar/hero.png){align="center"}

Des boutons personnalisés peuvent être ajoutés au **Barre d’outils de l’éditeur de texte enrichi** dans l’éditeur de fragment de contenu à l’aide de la méthode `rte` point d’extension. Cet exemple montre comment ajouter un bouton personnalisé appelé _Ajouter un conseil_ dans la barre d’outils de l’éditeur de texte enrichi et modifiez le contenu dans l’éditeur de texte enrichi.

Utilisation `rte` point d’extension `getCustomButtons()` méthode un ou plusieurs boutons personnalisés peuvent être ajoutés au **Barre d’outils de l’éditeur de texte enrichi**. Il est également possible d’ajouter ou de supprimer des boutons d’éditeur de texte enrichi standard tels que _Copier, coller, mettre en gras et mettre en italique_ using `getCoreButtons()` et `removeButtons)` les méthodes, respectivement.

Cet exemple montre comment insérer une note ou un conseil en surbrillance à l’aide d’une _Ajouter un conseil_ de la barre d’outils. Une mise en forme spéciale est appliquée au contenu de la note ou du conseil en surbrillance via les éléments de HTML et les classes CSS associées. Le contenu de l’espace réservé et le code du HTML sont insérés à l’aide de la fonction `onClick()` méthode de rappel de `getCustomButtons()`.

## Point d’extension

Cet exemple s’étend au point d’extension `rte` pour ajouter un bouton personnalisé à la barre d’outils de l’éditeur de fragment de contenu.

| Interface utilisateur AEM étendue | Point d’extension |
| ------------------------ | --------------------- | 
| [Éditeur de fragment de contenu](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Barre d’outils de l’éditeur de texte enrichi](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Exemple d’extension

L’exemple suivant crée une _Ajouter un conseil_ bouton personnalisé dans la barre d’outils de l’éditeur de texte enrichi. L’action de clic insère le texte de l’espace réservé à la position actuelle du signe d’insertion dans l’éditeur de texte enrichi.

Le code indique comment ajouter le bouton personnalisé à l’aide d’une icône et enregistrer la fonction de gestionnaire de clics.

### Enregistrement d’une extension

`ExtensionRegistration.js`, mappé à l’itinéraire index.html, est le point d’entrée de l’extension AEM et définit :

+ Définition du bouton de barre d’outils de l’éditeur de texte enrichi dans `getCustomButtons()` fonction avec `id, tooltip and icon` attributs.
+ Le gestionnaire de clics du bouton, dans la variable `onClick()` fonction .
+ La fonction de gestionnaire de clics reçoit la `state` comme argument pour obtenir le contenu de l’éditeur de texte enrichi au format HTML ou texte. Cependant, dans cet exemple, il n’est pas utilisé.
+ La fonction click handler renvoie un tableau d’instructions. Ce tableau comporte un objet avec `type` et `value` attributs. Pour insérer le contenu, le `value` Attribut le fragment de code de HTML, le `type` utilise l’attribut `insertContent`. Si un cas pratique de remplacement du contenu est présent, utilisez le cas suivant : `replaceContent` type d’instruction.

Le `insertContent` value est une chaîne de HTML, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. Classes CSS `cmp-contentfragment__element-tip` utilisé pour afficher la valeur ne sont pas définis dans le widget, mais plutôt implémentés sur l’expérience web sur laquelle ce champ Fragment de contenu s’affiche.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Toolbar custom button
          getCustomButtons: () => ([
            {
              id: "wknd-cf-tip",       // Provide a unique ID for the custom button
              tooltip: "Add Tip",      // Provide a label for the custom button
              icon: 'Note',            // Provide an icon for the button (see https://spectrum.adobe.com/page/icons/ for a list of available icons)
              onClick: (state) => {    // Provide a click handler function that returns the instructions array with type and value. This example inserts the HTML snippet for TIP content.
                return [{
                  type: "insertContent",
                  value: "<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>"
                }];
              },
            },
          ]),
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```
