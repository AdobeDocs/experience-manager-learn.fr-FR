---
title: Ajouter un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi
description: Découvrez comment ajouter un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi dans l’éditeur de fragment de contenu AEM.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13464
thumbnail: KT-13464.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 6fd93d3b-6d56-43c5-86e6-2e2685deecc9
duration: 354
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 95%

---

# Ajouter un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi

Découvrez comment ajouter un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi (RTE) dans l’éditeur de fragments de contenu AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3420768?quality=12&learn=on)

Des boutons personnalisés peuvent être ajoutés à la **barre d’outils de l’éditeur de texte enrichi** dans l’éditeur de fragment de contenu à l’aide du point d’extension `rte`. Cet exemple montre comment ajouter un bouton personnalisé appelé _Ajouter un conseil_ dans la barre d’outils de l’éditeur de texte enrichi et modifier le contenu dans l’éditeur de texte enrichi.

Grâce à la méthode `getCustomButtons()` du point d’extension `rte`, un ou plusieurs boutons personnalisés peuvent être ajoutés à la **barre d’outils de l’éditeur de texte enrichi**. Il est également possible d’ajouter ou de supprimer des boutons standard de l’éditeur de texte enrichi tels que _Copier, Coller, Mettre en gras et Mettre en italique_ à l’aide des méthodes `getCoreButtons()` et `removeButtons)`, respectivement.

Cet exemple montre comment insérer une note ou un conseil en surbrillance à l’aide du bouton personnalisé de la barre d’outils _Ajouter un conseil_. Une mise en forme spéciale est appliquée au contenu de la note ou du conseil en surbrillance via les éléments HTML et les classes CSS associées. Le contenu de l’espace réservé et le code du HTML sont insérés à l’aide de la méthode de rappel `onClick()` du `getCustomButtons()`.

## Point d’extension

Cet exemple s’étend au point d’extension `rte` pour ajouter un bouton personnalisé à la barre d’outils de l’éditeur de texte enrichi dans l’éditeur de fragment de contenu.

| Interface utilisateur AEM étendue | Point d’extension |
| ------------------------ | --------------------- | 
| [Éditeur de fragment de contenu](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [Barre d’outils de l’éditeur de texte enrichi](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/) |

## Exemple d’extension

L’exemple suivant crée un bouton personnalisé _Ajouter un conseil_ dans la barre d’outils de l’éditeur de texte enrichi. L’action de clic insère le texte de l’espace réservé à la position actuelle du caret dans l’éditeur de texte enrichi.

Le code indique comment ajouter le bouton personnalisé avec une icône et enregistrer la fonction de gestionnaire de clics.

### Enregistrement d’une extension

`ExtensionRegistration.js`, mappé à l’itinéraire index.html, est le point d’entrée de l’extension AEM et définit :

+ La définition du bouton de barre d’outils de l’éditeur de texte enrichi dans la fonction `getCustomButtons()` avec les attributs `id, tooltip and icon`.
+ Le gestionnaire de clics du bouton, dans la fonction `onClick()`.
+ La fonction de gestionnaire de clics reçoit l’objet `state` comme argument pour obtenir le contenu de l’éditeur de texte enrichi au format HTML ou texte. Toutefois, il n’est pas utilisé dans cet exemple.
+ La fonction de gestionnaire de clics renvoie un tableau d’instructions. Ce tableau comporte un objet avec les attributs `type` et `value`. Pour insérer le contenu, l’extrait de code HTML des attributs `value`, l’attribut `type` utilise `insertContent`. Pour remplacer le contenu, utilisez le type d’instruction `replaceContent`.

La valeur `insertContent` est une chaîne HTML, `<div class=\"cmp-contentfragment__element-tip\"><div>TIP</div><div>Add your tip text here...</div></div>`. Les classes CSS `cmp-contentfragment__element-tip` utilisées pour afficher la valeur ne sont pas définis dans le widget, mais plutôt implémentées sur l’expérience web sur laquelle ce champ de fragment de contenu s’affiche.


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
