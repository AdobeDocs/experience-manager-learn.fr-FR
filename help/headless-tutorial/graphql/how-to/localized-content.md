---
title: Utiliser du contenu localisé avec AEM Headless
description: Découvrez comment utiliser GraphQL pour envoyer une requête de contenu localisé à AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 100%

---

# Contenu localisé avec AEM Headless

AEM fournit une [Structure d’intégration de traduction](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html?lang=fr) pour le contenu découplé, ce qui permet aux fragments de contenu et aux ressources de prise en charge d’être facilement traduits pour une utilisation dans tous les paramètres régionaux. Il s’agit de la même structure que celle utilisée pour traduire d’autres contenus d’AEM, tels que des pages, des fragments d’expérience, des ressources et des formulaires. Une fois [le contenu découplé traduit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=fr) et publié, il est prêt à être utilisé par les applications découplées.

## Structure des dossiers de ressources{#assets-folder-structure}

Assurez-vous que les fragments de contenu localisés dans AEM suivent la [structure de localisation recommandée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html?lang=fr#recommended-structure).

![Dossiers de ressources AEM localisés.](./assets/localized-content/asset-folders.jpg)

Les dossiers de paramètres régionaux doivent être apparentés, et le nom du dossier, plutôt que son titre, doit être un [code ISO 639-1](https://fr.wikipedia.org/wiki/Liste_des_codes_ISO_639-1) valide représentant les paramètres régionaux du contenu du dossier.

Le code de paramètre régional est également la valeur utilisée pour filtrer les fragments de contenu renvoyés par la requête GraphQL.

| Code de paramètre régional | Chemin d’accès AEM | Paramètre régional du contenu |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Contenu en allemand |
| en | /content/dam/.../**en**/... | Contenu en anglais |
| es | /content/dam/.../**es**/... | Contenu en espagnol |

## Requête persistante GraphQL

AEM fournit un filtre GraphQL `_locale` qui filtre automatiquement le contenu par code de paramètre régional. Par exemple, il est possible d’interroger toutes les aventures en anglais dans le [projet WKND Site](https://github.com/adobe/aem-guides-wknd) à l’aide d’une nouvelle requête persistante `wknd-shared/adventures-by-locale` définie comme :

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

La variable `$locale` utilisée dans le filtre `_locale` nécessite le code de paramètre régional (par exemple `en`, `en_us`ou `de`), comme indiqué dans la [convention de localisation de base des dossiers de ressources AEM](#assets-folder-structure).

## Exemple pour React

Créons une application React simple qui contrôle le contenu d’Adventure à interroger à partir d’AEM en fonction d’un sélecteur de paramètre régional à l’aide du filtre `_locale`.

Lorsque vous sélectionnez l’__anglais__ dans le sélecteur de paramètre régional, les fragments de contenu d’Adventure en anglais sous `/content/dam/wknd/en` sont renvoyés. Lorsque vous sélectionnez l’__espagnol__, les fragments de contenu en espagnol sous `/content/dam/wknd/es` sont renvoyés, etc.

![Exemple d’application React localisée.](./assets/localized-content/react-example.png)

### Créer un `LocaleContext`{#locale-context}

Tout d’abord, créez un [contexte React](https://fr.reactjs.org/docs/context.html) pour permettre l’utilisation des paramètres régionaux dans les composants de l’application React.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### Créer un composant React `LocaleSwitcher`{#locale-switcher}

Ensuite, créez un composant React de sélecteur de paramètre régional qui définit la valeur de [LocaleContext](#locale-context) en fonction de la sélection de l’utilisateur ou de l’utilisatrice.

Cette valeur locale est utilisée pour orienter les requêtes GraphQL, en s’assurant qu’elles renvoient uniquement le contenu correspondant au paramètre régional sélectionné.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### Interroger le contenu à l’aide du filtre `_locale`{#adventures}

Le composant Adventures interroge AEM sur toutes les Adventures en fonction du paramètre régional et répertorie leurs titres. Pour ce faire, transmettez la valeur du paramètre régional stockée dans le contexte React à la requête à l’aide du filtre `_locale`.

Cette approche peut s’appliquer à d’autres requêtes de votre application, à condition que toutes les requêtes incluent uniquement le contenu spécifié par la sélection des paramètre régional d’un utilisateur ou d’une utilisatrice.

L’interrogation d’AEM est effectuée dans le hook React personnalisé [getAdventuresByLocale, sur lequel vous trouverez davantage d’informations dans la documentation GraphQL sur l’interrogation d’AEM](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### Définir le `App.js`{#app-js}

Enfin, unissez l’ensemble en encapsulant l’application React avec le `LanguageContext.Provider` et en définissant la valeur du paramètre régional. Cela permet aux autres composants React, [LocaleSwitcher](#locale-switcher), et [Adventures](#adventures) de partager l’état de sélection du paramètre régional.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
