---
title: Aperçu du fragment de contenu
description: Découvrez comment utiliser l’aperçu de fragment de contenu pour tous les auteurs afin de voir rapidement comment les modifications de contenu affectent vos expériences AEM sans affichage.
version: Cloud Service
feature: Content Fragments
topic: Headless, Content Management, Development
role: Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-03-17T00:00:00Z
jira: KT-10841
thumbnail: 3416906.jpeg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---


# Aperçu du fragment de contenu

Les applications AEM sans affichage prennent en charge la prévisualisation de création intégrée. L’expérience d’aperçu relie l’éditeur de fragment de contenu de l’auteur AEM à votre application personnalisée (accessible via HTTP), ce qui permet d’insérer un lien profond dans l’application qui effectue le rendu du fragment de contenu en cours de prévisualisation.

>[!VIDEO](https://video.tv.adobe.com/v/3416906?quality=12&learn=on)

Pour utiliser l’aperçu du fragment de contenu, plusieurs conditions doivent être remplies :

1. L’application doit être déployée vers une URL accessible aux auteurs.
1. L’application doit être configurée pour se connecter au service AEM Author (plutôt qu’au service AEM Publish).
1. L’application doit être conçue avec des URL ou des itinéraires pouvant utiliser [Chemin ou identifiant du fragment de contenu](#url-expressions) pour sélectionner les fragments de contenu à afficher en vue d’un aperçu dans l’expérience de l’application.

## URL d’aperçu

URL d’aperçu, à l’aide de [Expressions d’URL](#url-expressions), sont définis sur les propriétés du modèle de fragment de contenu.

![URL d’aperçu du modèle de fragment de contenu](./assets/preview/cf-model-preview-url.png)

1. Connectez-vous au service Auteur AEM en tant qu’administrateur.
1. Accédez à __Outils > Général > Modèles de fragment de contenu__
1. Sélectionnez la __Modèle de fragment de contenu__ et sélectionnez __Propriétés__ dans la barre d’actions supérieure.
1. Saisissez l’URL d’aperçu du modèle de fragment de contenu à l’aide de [Expressions d’URL](#url-expressions)
   + L’URL d’aperçu doit pointer vers un déploiement de l’application qui se connecte au service AEM Author.

### Expressions d’URL

Un URL d’aperçu peut être défini pour chaque modèle de fragment de contenu. L’URL d’aperçu peut être paramétrée par fragment de contenu à l’aide des expressions d’URL répertoriées dans le tableau ci-dessous. Plusieurs expressions d’URL peuvent être utilisées dans une seule URL d’aperçu.

|  | Expression d’URL | Valeur  |
| --------------------------------------- | ----------------------------------- | ----------- |
| Chemin du fragment de contenu | `${contentFragment.path}` | `/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali` |
| Identifiant du fragment de contenu | `${contentFragment.id}` | `12c34567-8901-2aa3-45b6-d7890aa1c23c` |
| Variation de fragment de contenu | `${contentFragment.variation}` | `main` |
| Chemin du modèle de fragment de contenu | `${contentFragment.model.path}` | `/conf/wknd-shared/settings/dam/cfm/models/adventure` |
| Nom du modèle de fragment de contenu | `${contentFragment.model.name}` | `adventure` |

Exemples d’URL d’aperçu :

+ Une URL d’aperçu sur la __Adventure__ modèle pourrait ressembler à `https://preview.app.wknd.site/adventure${contentFragment.path}` qui résout sur `https://preview.app.wknd.site/adventure/content/dam/wknd-shared/en/adventures/surf-camp-bali/surf-camp-bali`
+ Une URL d’aperçu sur la __Article__ modèle pourrait ressembler à `https://preview.news.wknd.site/${contentFragment.model.name}/${contentFragment.id}.html?variation=${contentFragment.variation}` la résolution `https://preview.news.wknd.site/article/99c34317-1901-2ab3-35b6-d7890aa1c23c.html?variation=main`

## Aperçu in-app

Tout fragment de contenu utilisant le modèle de fragment de contenu configuré comporte un bouton Aperçu . Le bouton Aperçu ouvre l’URL d’aperçu du modèle de fragment de contenu et injecte les valeurs du fragment de contenu ouvert dans la variable [Expressions d’URL](#url-expressions).

![Bouton Aperçu](./assets/preview/preview-button.png)

Effectuez une actualisation stricte (effacer le cache local du navigateur) lors de la prévisualisation des modifications du fragment de contenu dans l’application.

## Exemple React

Explorons l’application WKND, une application React simple qui présente les aventures d’AEM en utilisant AEM API GraphQL sans affichage.

L’exemple de code est disponible sur [Github.com](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/preview-tutorial).

## URL et itinéraires

Les URL ou les itinéraires utilisés pour prévisualiser un fragment de contenu doivent pouvoir être composites à l’aide de [Expressions d’URL](#url-expressions). Dans cette version de l’application WKND avec prévisualisation, les fragments de contenu aventure s’affichent via le `AdventureDetail` composant lié à l’itinéraire `/adventure<CONTENT FRAGMENT PATH>`. Par conséquent, l’URL d’aperçu du modèle WKND Adventure doit être définie sur `https://preview.app.wknd.site:3000/adventure${contentFragment.path}` pour résoudre ce problème.

L’aperçu de fragment de contenu ne fonctionne que si l’application dispose d’un itinéraire adressable pouvant être renseigné par [Expressions d’URL](#url-expressions) qui restituent ce fragment de contenu dans l’application d’une manière prévisualisable.

+ `src/App.js`

```javascript
...
function App() {
  return (
    <Router>
      <div className="App">
        <header>
            <Link to={"/"}>
                <img src={logo} className="logo" alt="WKND Logo"/>
            </Link>        
            <hr />
        </header>
        <Routes>
          {/* The route's path must match the Adventure Model's Preview URL expression. In React since the path has `/` you must use wildcards to match instead of the usual `:path` */}
          <Route path='/adventure/*' element={<AdventureDetail />}/>
          <Route path="/" element={<Home />}/>
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

### Afficher le contenu créé

Le `AdventureDetail` Le composant analyse simplement le chemin d’accès au fragment de contenu, injecté dans l’URL d’aperçu via la propriété `${contentFragment.path}` [Expression d’URL](#url-expressions), à partir de l’URL de l’itinéraire et l’utilise pour collecter et effectuer le rendu de WKND Adventure.

+ `src/components/AdventureDetail.js`

```javascript
...
function AdventureDetail() {

    // Read the `path` value which is the parameter used to query for the adventure's details
    // since the params value captures the `*` wildcard in `/adventure/*`, or everything after the first `/` in the Content Fragment path.
    const params = useParams();
    const pathParam = params["*"];

    // Add the leading '/' back on 
    const path = '/' + pathParam;
    
    // Query AEM for the Adventures's details, using the Content Fragment's `path`
    const { adventure, references, error } = useAdventureByPath(path);

    // Handle error and loading conditions
    if (error) {
        return <Error errorMessage={error} />;
    } else if (!adventure) {
        return <Loading />;
    }

    return (<div className="adventure-detail">
        ...
        <AdventureDetailRender {...adventure} references={references} />
    </div>);
}
...
```
