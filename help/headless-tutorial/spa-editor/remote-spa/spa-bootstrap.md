---
title: Bootstrap de la SPA distante pour SPA Editor
description: Découvrez comment amorcer un SPA distant pour AEM compatibilité avec l’éditeur d’SPA.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1285'
ht-degree: 3%

---

# Bootstrap de la SPA distante pour SPA Editor

Avant que les zones modifiables puissent être ajoutées au SPA distant, il doit être démarré avec le SDK JavaScript de l’AEM éditeur et quelques autres configurations.


## Téléchargement de la source de l’application WKND

Si vous ne l’avez pas déjà fait, téléchargez le code source de l’application WKND à partir de Github.com, puis changez de branche contenant les modifications apportées au SPA exécuté dans ce tutoriel.

```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ git clone --branch feature/spa-editor https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd aem-guides-wknd-graphql
```

## Examinez les AEM dépendances npm du SDK JS de l’éditeur SPA

Commencez par examiner AEM dépendances SPA npm du projet React.

+ [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : fournit l’API pour récupérer du contenu d’AEM.
+ [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : fournit l’API qui mappe AEM contenu aux composants SPA.
+ [`@adobe/aem-react-editable-components`](https://github.com/adobe/aem-react-editable-components) : fournit une API pour la création de composants SPA personnalisés et fournit des implémentations courantes telles que le `AEMPage` Composant React.
+ [`@adobe/aem-core-components-react-base`](https://github.com/adobe/aem-react-core-wcm-components-base) : fournit une suite de composants React prêts à l’emploi qui s’intègrent facilement aux composants principaux de la gestion de contenu web d’AEM et sont indifférents SPA l’éditeur. Il s’agit principalement de composants de contenu, tels que :
   + Titre
   + Texte
   + Chemin de navigation
   + Et ainsi de suite.
+ [`@adobe/aem-core-components-react-spa`](https://github.com/adobe/aem-react-core-wcm-components-spa) : fournit une suite de composants React prêts à l’emploi qui s’intègrent facilement aux composants principaux de la gestion de contenu web d’AEM et qui nécessitent SPA éditeur. Il contient principalement des composants qui contiennent des composants de contenu issus de `@adobe/aem-core-components-react-base`, par exemple :
   + Conteneur
   + Carrousel
   + etc.

## Vérification SPA variables d’environnement

Plusieurs variables d’environnement doivent être exposées à la SPA distante afin qu’elle sache interagir avec AEM.

1. Ouvrez le projet SPA à distance à l’adresse `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` dans votre IDE
1. Ouvrez le fichier `.env.development`
1. Dans le fichier , prêtez une attention particulière aux clés :

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   
   REACT_APP_USE_PROXY=true
   
   REACT_APP_AUTH_METHOD=basic
   
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

   ![Variables d’environnement SPA distantes](./assets/spa-bootstrap/env-variables.png)

   *N’oubliez pas que les variables d’environnement personnalisées dans React doivent comporter le préfixe `REACT_APP_`.*

   + `REACT_APP_HOST_URI`: le schéma et l’hôte du service AEM auquel le SPA distant se connecte.
      + Cette valeur change selon que l’environnement AEM (local, de développement, d’évaluation ou de production) et le type de service AEM (auteur ou publication).
   + `REACT_APP_USE_PROXY`: cela évite les problèmes CORS pendant le développement en informant le serveur de développement de la réaction des requêtes d’AEM proxy telles que `/content, /graphql, .model.json` using `http-proxy-middleware` module .
   + `REACT_APP_AUTH_METHOD`: méthode d’authentification pour les demandes AEM diffusées, les options sont &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; ou laissez vide pour un cas d’utilisation sans authentification
      + Requis pour une utilisation avec l’auteur AEM
      + Possibilité d’utilisation avec AEM Publish (si le contenu est protégé)
      + Le développement par rapport au SDK AEM prend en charge les comptes locaux via l’authentification de base. Il s’agit de la méthode utilisée dans ce tutoriel.
      + Lors de l’intégration à AEM as a Cloud Service, utilisez [jetons d’accès](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
   + `REACT_APP_BASIC_AUTH_USER`: l’AEM __username__ par le SPA à authentifier lors de la récupération du contenu AEM.
   + `REACT_APP_BASIC_AUTH_PASS`: l’AEM __password__ par le SPA à authentifier lors de la récupération du contenu AEM.

## Intégration de l’API ModelManager

Avec les dépendances SPA npm d’AEM disponibles pour l’application, initialisez l’AEM `ModelManager` dans le fichier `index.js` before `ReactDOM.render(...)` est appelée.

Le [ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts) est chargé de se connecter à AEM pour récupérer le contenu modifiable.

1. Ouvrez le projet SPA distant dans votre IDE.
1. Ouvrez le fichier `src/index.js`
1. Ajouter un import `ModelManager` et l’initialisez avant l’événement `ReactDOM.render(..)` appel,

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

Le `src/index.js` doit se présenter comme suit :

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Configuration d’un proxy SPA interne

Lors de l’approvisionnement de contenu modifiable à partir d’AEM dans le SPA, il est préférable de configurer une [proxy interne dans le SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), qui est configuré pour acheminer les requêtes appropriées vers AEM. Pour ce faire, utilisez [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) module npm, qui est déjà installé par l’application WKND GraphQL de base.

1. Ouvrez le projet SPA distant dans votre IDE.
1. Ouvrez le fichier  dans `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Examinez le code suivant :

   ```
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphql') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure:') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: REACT_APP_AUTHORIZATION,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   Le `setupProxy.spa-editor.auth.basic.js` doit se présenter comme suit :

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Cette configuration de proxy effectue deux opérations principales :

   1. Requêtes spécifiques aux proxys effectuées au SPA, `http://localhost:3000` à AEM `http://localhost:4502`
      + Il ne s’agit que des requêtes de proxy dont les chemins correspondent à des modèles qui indiquent qu’ils doivent être diffusés par AEM, comme défini dans `toAEM(path, req)`.
      + Il réécrit SPA chemins d’accès aux pages AEM correspondantes, comme défini dans `pathRewriteToAEM(path, req)`
   1. Il ajoute des en-têtes CORS à toutes les requêtes pour permettre l’accès au contenu AEM, tel que défini par `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + Si ce n’est pas le cas, des erreurs CORS se produisent lors du chargement AEM contenu dans le SPA.

1. Ouvrez le fichier `src/setupProxy.js`
1. Examinez la ligne pointant vers le `setupProxy.spa-editor.auth.basic` fichier de configuration du proxy :

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

   Le `setupProxy.js` doit se présenter comme suit :

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

Notez que toute modification apportée à la variable `src/setupProxy.js` ou les fichiers référencés nécessitent un redémarrage de la SPA.

## Ressource SPA statique

Les ressources de SPA statiques telles que le logo WKND et le graphique de chargement doivent avoir leurs URL src mises à jour pour les forcer à charger à partir de l’hôte SPA distant. S’il reste relatif, lorsque le SPA est chargé dans SPA Éditeur pour la création, ces URL utilisent par défaut l’hôte de l’instance d’envoi plutôt que le , ce qui génère 404 requêtes, comme illustré dans l’image ci-dessous.

![Ressources statiques endommagées](./assets/spa-bootstrap/broken-static-resource.png)

Pour résoudre ce problème, faites en sorte qu’une ressource statique hébergée par la SPA distante utilise des chemins absolus qui incluent l’origine SPA distante.

1. Ouvrez le projet SPA dans votre IDE.
1. Ouvrez votre fichier de variables d’environnement SPA `src/.env.development` et ajoutez une variable pour l’URI public SPA :

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Lors d’un déploiement sur AEM as a Cloud Service, vous devez procéder de la même manière pour la variable `.env` fichiers ._

1. Ouvrez le fichier `src/App.js`
1. Importez l’URI public SPA à partir des variables d’environnement SPA

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Préfixe le logo WKND `<img src=.../>` avec `REACT_APP_PUBLIC_URI` pour forcer la résolution contre le SPA.

   ```
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. Faites de même pour charger l’image dans `src/components/Loading.js`

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. Et pour le __deux instances__ du bouton Précédent dans `src/components/AdventureDetails.js`

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Le `App.js`, `Loading.js`, et `AdventureDetails.js` Les fichiers doivent se présenter comme suit :

![Ressources statiques](./assets/spa-bootstrap/static-resources.png)

## AEM Grille réactive

Pour prendre en charge le mode de mise en page de SPA Éditeur pour les zones modifiables dans la SPA, nous devons intégrer le code CSS Grille réactive de l’AEM dans le . Ne vous inquiétez pas : ce système de grille ne s’applique qu’aux conteneurs modifiables et vous pouvez utiliser votre système de grille de votre choix pour orienter la mise en page du reste de votre SPA.

Ajoutez les AEM fichiers SCSS de grille réactive à la SPA.

1. Ouvrez le projet SPA dans votre IDE.
1. Téléchargez et copiez les deux fichiers suivants dans `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + Générateur SCSS de grille réactive AEM
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Appels `_grid.scss` à l’aide des points d’arrêt spécifiques SPA (ordinateurs de bureau et mobiles) et des colonnes (12).
1. Ouvrir `src/App.scss` et import `./styles/grid-init.scss`

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

Le `_grid.scss` et `_grid-init.scss` Les fichiers doivent se présenter comme suit :

![AEM SCSS à grille réactive](./assets/spa-bootstrap/aem-responsive-grid.png)

Désormais, le SPA inclut le CSS requis pour prendre en charge AEM mode Mise en page pour les composants ajoutés à un conteneur d’AEM.

## Démarrez le SPA

Maintenant que le SPA est amorcé pour l’intégration à AEM, faisons tourner le  et voyons à quoi ça ressemble !

1. Sur la ligne de commande, accédez à la racine du projet SPA
1. Démarrez le SPA à l’aide des commandes normales (si vous ne l’avez pas déjà fait).

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. Parcourir le SPA [http://localhost:3000](http://localhost:3000). Tout devrait être beau !

![SPA s’exécutant sur http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Ouvrez le SPA dans AEM Éditeur

Avec le SPA en cours d’exécution [http://localhost:3000](http://localhost:3000), nous allons l’ouvrir à l’aide d’AEM SPA Editor. Rien n’est encore modifiable dans le SPA, cela ne valide que le SPA dans l’.

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND > us > en__
1. Sélectionnez la __Page d’accueil de l’application WKND__ et appuyez sur __Modifier__ et le SPA apparaît.

   ![Modifier la page d’accueil de l’application WKND](./assets/spa-bootstrap/edit-home.png)

1. Basculer vers __Aperçu__ utilisation du sélecteur de mode en haut à droite
1. Cliquez sur le SPA

   ![SPA s’exécutant sur http://localhost:3000](./assets/spa-bootstrap/spa-editor.png)

## Félicitations !

Vous avez amorcé la SPA distante pour être AEM Éditeur compatible ! Vous savez maintenant comment :

+ Ajoutez les dépendances npm du SDK JS de l’AEM Éditeur SPA au projet SPA
+ Configuration de vos variables d’environnement SPA
+ Intégration de l’API ModelManager à la SPA
+ Configurez un proxy interne pour le SPA afin qu’il achemine les demandes de contenu appropriées vers AEM
+ Résolution des problèmes liés aux ressources SPA statiques dans le contexte de SPA Editor
+ Ajout d’AEM CSS à grille réactive pour prendre en charge la mise en page dans AEM conteneurs modifiables

## Étapes suivantes

Maintenant que nous avons atteint une base de compatibilité avec AEM SPA Editor, nous pouvons commencer à introduire des zones modifiables. Nous allons d’abord examiner comment placer une [composant modifiable fixe](./spa-fixed-component.md) dans le SPA.
