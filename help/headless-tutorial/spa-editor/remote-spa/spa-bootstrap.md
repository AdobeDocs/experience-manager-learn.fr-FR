---
title: Bootstrap de la SPA distante pour SPA Editor
description: Découvrez comment amorcer un SPA distant pour AEM compatibilité avec l’éditeur d’SPA.
topic: Sans tête, SPA, développement
feature: Éditeur SPA, composants principaux, API, développement
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
source-git-commit: 5dea9cf646762c0f4aff43d9e48a35ab6ebc0af8
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 2%

---


# Bootstrap de la SPA distante pour SPA Editor

Avant que les zones modifiables puissent être ajoutées au SPA distant, il doit être démarré avec le SDK JavaScript de l’AEM éditeur et quelques autres configurations.

## Ajout AEM dépendances npm du SDK JS de l’éditeur SPA

Tout d’abord, ajoutez AEM dépendances npm SPA au projet React.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install --save \
    @adobe/aem-spa-page-model-manager \
    @adobe/aem-spa-component-mapping \
    @adobe/aem-react-editable-components \
    @adobe/aem-core-components-react-base \
    @adobe/aem-core-components-react-spa
```

+ `@adobe/aem-spa-page-model-manager` fournit l’API pour récupérer du contenu d’AEM.
+ `@adobe/aem-spa-component-mapping` fournit l’API qui mappe AEM contenu aux composants SPA.
+ ` @adobe/aem-react-editable-components` fournit une API pour la création de composants SPA personnalisés et fournit des implémentations courantes telles que le composant  `AEMPage` React.
+ `@adobe/aem-core-components-react-base` fournit une suite de composants React prêts à l’emploi qui s’intègrent facilement aux composants principaux de la gestion de contenu web d’AEM et sont indifférents SPA l’éditeur. Il s’agit principalement de composants de contenu, tels que :
   + Titre
   + Texte
   + Chemin de navigation
   + Et ainsi de suite.
+ `@adobe/aem-core-components-react-spa` fournit une suite de composants React prêts à l’emploi qui s’intègrent facilement aux composants principaux de la gestion de contenu web d’AEM et qui nécessitent SPA éditeur. Ils contiennent principalement des composants qui contiennent des composants de contenu provenant de `@adobe/aem-core-components-react-base`, tels que :
   + Conteneur
   + Carrousel
   + etc.

## Vérification SPA variables d’environnement

Plusieurs variables d’environnement doivent être exposées à la SPA distante afin qu’elle sache interagir avec AEM.

1. Ouvrez le projet SPA distant à `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` dans votre IDE.
1. Ouvrez le fichier `.env.development`
1. Ajoutez le fichier en portant une attention particulière aux clés :

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   ![Variables d’environnement SPA distantes](./assets/spa-bootstrap/env-variables.png)

   *N’oubliez pas que les variables d’environnement personnalisées dans React doivent comporter le préfixe  `REACT_APP_`.*

   + `REACT_APP_AEM_URI`: le schéma et l’hôte du service AEM auquel le SPA distant se connecte.
      + Cette valeur change selon que l’environnement AEM (local, de développement, d’évaluation ou de production) et le type de service AEM (auteur ou publication).
   + `REACT_APP_AEM_AUTH`: les informations d’identification utilisées par SPA s’authentifient pour AEM et récupèrent du contenu.
      + Requis pour une utilisation avec l’auteur AEM
      + Possibilité d’utilisation avec AEM Publish (si le contenu est protégé)
      + Le développement par rapport au SDK AEM prend en charge les comptes locaux via l’authentification de base. Il s’agit de la méthode utilisée dans ce tutoriel.
      + Lors de l’intégration à AEM en tant que Cloud Service, utilisez [jetons d’accès](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)

## Intégration de l’API ModelManager

Avec les dépendances npm d’AEM disponibles pour l’application, initialisez l’élément `ModelManager` dans la balise `index.js` du projet avant l’appel de `ReactDOM.render(...)`.

[ModelManager](https://www.npmjs.com/package/@adobe/aem-spa-page-model-manager) est chargé de se connecter à AEM pour récupérer le contenu modifiable.

1. Ouvrez le projet SPA distant dans votre IDE.
1. Ouvrez le fichier `src/index.js`
1. Ajoutez l’importation `ModelManager` et initialisez-la avant l’appel `ReactDOM.render(..)`,

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

Le fichier `src/index.js` doit se présenter comme suit :

![src/index.js](./assets/spa-bootstrap/index-js.png)

## Configuration d’un proxy SPA interne

Lors de l’approvisionnement de contenu modifiable à partir d’AEM dans la SPA, il est préférable de configurer un [proxy interne dans la balise ](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually), qui est configuré pour acheminer les requêtes appropriées vers. Pour ce faire, utilisez le module npm [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) qui est déjà installé par l’application graphique WKND de base.

1. Ouvrez le projet SPA distant dans votre IDE.
1. Créez un fichier sur `src/proxy/setupProxy.spa-editor.auth.basic.js`
1. Ajoutez le code suivant au fichier :

   ```
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_AUTHORIZATION } = process.env;
   
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
               path.startsWith('/graphq') ||
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

   Le fichier `setupProxy.spa-editor.auth.basic.js` doit se présenter comme suit :

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   Cette configuration de proxy effectue deux opérations principales :

   1. Requêtes spécifiques aux proxys effectuées au SPA, `http://localhost:3000` pour AEM `http://localhost:4502`
      + Il ne crée que des proxys dont les chemins correspondent à des modèles qui indiquent qu’ils doivent être diffusés par AEM, comme défini dans `toAEM(path, req)`.
      + Il réécrit SPA chemins d’accès à leurs pages AEM de contrepartie, comme défini dans `pathRewriteToAEM(path, req)`
   1. Il ajoute des en-têtes CORS à toutes les demandes pour permettre l’accès au contenu AEM, comme défini par `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`
      + Si ce n’est pas le cas, des erreurs CORS se produisent lors du chargement AEM contenu dans le SPA.

1. Ouvrez le fichier `src/setupProxy.js`
1. Mettre la ligne en commentaire `const proxy = require('./proxy/setupProxy.auth.basic')`
1. Ajoutez une ligne pointant vers le nouveau fichier de configuration du proxy :

   ```
   // Proxy configuration for SPA Editor (and GraphQL) using Basic Auth
   const proxy = require('./proxy/setupProxy.spa-editor.auth.basic')
   ```

   Le fichier `setupProxy.js` doit se présenter comme suit :

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

Notez que toute modification apportée à `src/setupProxy.js` ou à ses fichiers référencés nécessite un redémarrage du SPA.

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

   _Lors d’un déploiement vers AEM en tant que Cloud Service, vous devez en faire de même pour les  `.env` fichiers correspondants._

1. Ouvrez le fichier `src/App.js`
1. Importez l’URI public SPA à partir des variables d’environnement SPA

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. Préfixez le logo WKND `<img src=.../>` avec `REACT_APP_PUBLIC_URI` pour forcer la résolution de la SPA.

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

1. . et pour les __deux instances__ du bouton Précédent dans `src/components/AdventureDetails.js`

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

Les fichiers `App.js`, `Loading.js` et `AdventureDetails.js` doivent se présenter comme suit :

![Ressources statiques](./assets/spa-bootstrap/static-resources.png)

## AEM Grille réactive

Pour prendre en charge le mode de mise en page de SPA Editor pour les zones modifiables de l’SPA, nous devons intégrer le code CSS Grille réactive d’AEM dans le . Ne vous inquiétez pas : ce système de grille s’applique uniquement aux conteneurs modifiables. Vous pouvez utiliser votre système de grille de votre choix pour orienter la mise en page du reste de votre SPA.

Ajoutez les AEM fichiers SCSS de grille réactive à la SPA.

1. Ouvrez le projet SPA dans votre IDE.
1. Téléchargez et copiez les deux fichiers suivants dans `src/styles`
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + Générateur SCSS de grille réactive AEM
   + [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      + Appelle `_grid.scss` à l’aide des points d’arrêt spécifiques SPA (ordinateurs de bureau et appareils mobiles) et des colonnes (12).
1. Ouvrez `src/App.scss` et importez `./styles/grid-init.scss`

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

Les fichiers `_grid.scss` et `_grid-init.scss` doivent se présenter comme suit :

![AEM SCSS à grille réactive](./assets/spa-bootstrap/aem-responsive-grid.png)

Désormais, le SPA inclut le CSS requis pour prendre en charge AEM mode Mise en page pour les composants ajoutés à un conteneur d’AEM.

## Démarrez le SPA

Maintenant que le SPA est amorcé pour l’intégration à AEM, faisons tourner le  et voyons à quoi ça ressemble !

1. Sur la ligne de commande, accédez à la racine du projet SPA
1. Démarrez le SPA à l’aide des commandes normales (exécutez `npm install` si ce n’est pas déjà fait).

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. Parcourez le SPA sur [http://localhost:3000](http://localhost:3000). Tout devrait être beau !

![SPA s’exécutant sur http://localhost:3000](./assets/spa-bootstrap/localhost-3000.png)

## Ouvrez le SPA dans AEM Éditeur

Avec le SPA exécuté sur [http://localhost:3000](http://localhost:3000), ouvrons-le à l’aide d’AEM Éditeur. Rien n’est encore modifiable dans le SPA, cela ne valide que le SPA dans l’.

1. Connexion à l’auteur AEM
1. Accédez à __Sites > Application WKND > us > en__
1. Sélectionnez la __page d’accueil de l’application WKND__, appuyez sur __Modifier__ et le SPA s’affiche.

   ![Modifier la page d’accueil de l’application WKND](./assets/spa-bootstrap/edit-home.png)

1. Basculez vers __Aperçu__ à l’aide du sélecteur de mode en haut à droite.
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

Maintenant que nous avons atteint une base de compatibilité avec AEM SPA Editor, nous pouvons commencer à introduire des zones modifiables. Nous allons d’abord examiner comment placer un [composant modifiable fixe](./spa-fixed-component.md) dans le SPA.
