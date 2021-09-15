---
title: Ajout de la navigation et du routage | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de la version de la suite de rapports. La navigation dynamique est mise en oeuvre à l’aide des composants principaux React Router et React.
sub-product: sites
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1619'
ht-degree: 0%

---

# Ajout de la navigation et du routage {#navigation-routing}

Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de la version de la suite de rapports. La navigation dynamique est mise en oeuvre à l’aide des composants principaux React Router et React.

## Objectif

1. Découvrez les options de routage du modèle SPA disponibles lors de l’utilisation de SPA Editor.
1. Découvrez comment utiliser [le routeur de réaction](https://reacttraining.com/react-router/) pour naviguer entre les différentes vues de la SPA.
1. Utilisez AEM composants principaux React pour mettre en oeuvre une navigation dynamique pilotée par la hiérarchie de pages AEM.

## Ce que vous allez créer

Ce chapitre ajoute une navigation à un SPA dans AEM. Le menu de navigation sera piloté par la hiérarchie AEM page et utilisera le modèle JSON fourni par le [composant principal de navigation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigation ajoutée](assets/navigation-routing/navigation-added.png)

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Mapper les composants](map-components.md). Toutefois, pour suivre, vous avez besoin d’un projet AEM activé SPA déployé sur une instance d’AEM locale.

## Ajouter la navigation au modèle {#add-navigation-template}

1. Ouvrez un navigateur et connectez-vous à AEM, [http://localhost:4502/](http://localhost:4502/). La base de code de départ doit déjà être déployée.
1. Accédez au **Modèle de page SPA** : [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Sélectionnez le **conteneur de mise en page racine le plus éloigné** et cliquez sur son icône **Stratégie**. Veillez à **ne pas** sélectionner le **conteneur de mises en page** déverrouillé pour la création.

   ![Icône de stratégie de conteneur de mises en page racine](assets/navigation-routing/root-layout-container-policy.png)

1. Créez une nouvelle stratégie nommée **SPA Structure** :

   ![Stratégie de structure SPA](assets/navigation-routing/spa-policy-update.png)

   Sous **Composants autorisés** > **Général** > sélectionnez le composant **Conteneur de mises en page** .

   Sous **Composants autorisés** > **WKND SPA REACT - STRUCTURE** > sélectionnez le composant **Navigation** :

   ![Sélectionner le composant Navigation](assets/navigation-routing/select-navigation-component.png)

   Sous **Composants autorisés** > **WKND SPA REACT - Content** > sélectionnez les composants **Image** et **Texte**. Vous devez avoir 4 composants au total sélectionnés.

   Cliquez sur **Terminé** pour enregistrer les modifications.

1. Actualisez la page, puis ajoutez le composant **Navigation** au-dessus du **Conteneur de mises en page déverrouillé** :

   ![ajouter le composant Navigation au modèle](assets/navigation-routing/add-navigation-component.png)

1. Sélectionnez le composant **Navigation** et cliquez sur son icône **Stratégie** pour modifier la stratégie.
1. Créez une nouvelle stratégie avec un **Titre de la stratégie** de **SPA Navigation**.

   Sous **Propriétés** :

   * Définissez la **racine de navigation** sur `/content/wknd-spa-react/us/en`.
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Décochez **Collecter toutes les pages enfants**.
   * Définissez la **Profondeur de la structure de navigation** sur **3**.

   ![Configuration de la stratégie de navigation](assets/navigation-routing/navigation-policy.png)

   Cette opération collecte les 2 niveaux de navigation profonds sous `/content/wknd-spa-react/us/en`.

1. Après avoir enregistré vos modifications, le `Navigation` renseigné doit s’afficher dans le modèle :

   ![Composant de navigation renseigné](assets/navigation-routing/populated-navigation.png)

## Créer des pages enfants

Créez ensuite des pages supplémentaires dans AEM qui serviront de vues différentes dans le SPA. Nous examinerons également la structure hiérarchique du modèle JSON fourni par AEM.

1. Accédez à la console **Sites** : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Sélectionnez **Page d’accueil de React SPA WKND** et cliquez sur **Créer** > **Page** :

   ![Créer une page](assets/navigation-routing/create-new-page.png)

1. Sous **Modèle**, sélectionnez **SPA Page**. Sous **Propriétés**, saisissez **Page 1** pour le **titre** et **page-1** comme nom.

   ![Renseignez les propriétés initiales de la page](assets/navigation-routing/initial-page-properties.png)

   Cliquez sur **Créer** puis, dans la fenêtre contextuelle de la boîte de dialogue, cliquez sur **Ouvrir** pour ouvrir la page dans l’éditeur SPA d’AEM.

1. Ajoutez un nouveau composant **Texte** au **Conteneur de mises en page** principal. Modifiez le composant et saisissez le texte : **Page 1** à l’aide de l’éditeur de texte enrichi et de l’élément **H2**.

   ![Exemple de contenu page 1](assets/navigation-routing/page-1-sample-content.png)

   N’hésitez pas à ajouter du contenu supplémentaire, comme une image.

1. Revenez à la console AEM Sites et répétez les étapes ci-dessus, en créant une seconde page nommée **Page 2** en tant que frère de **Page 1**.
1. Enfin, créez une troisième page, **Page 3** mais en tant que **enfant** de **Page 2**. Une fois la hiérarchie du site terminée, elle doit se présenter comme suit :

   ![Exemple de hiérarchie de site](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Le composant Navigation peut désormais être utilisé pour accéder à différentes zones du SPA.

   ![Navigation et routage](assets/navigation-routing/navigation-working.gif)

1. Ouvrez la page en dehors de l’éditeur d’AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Utilisez le composant **Navigation** pour accéder à différentes vues de l’application.

1. Utilisez les outils de développement de votre navigateur pour examiner les requêtes réseau au fur et à mesure de votre navigation. Les captures d’écran ci-dessous sont capturées à partir du navigateur Google Chrome.

   ![Observez les requêtes réseau](assets/navigation-routing/inspect-network-requests.png)

   Notez qu’après le chargement initial de la page, la navigation suivante n’entraîne pas une actualisation complète de la page et que le trafic réseau est réduit lors du retour aux pages précédemment visitées.

## Modèle JSON de page de hiérarchie {#hierarchy-page-json-model}

Examinez ensuite le modèle JSON qui anime l’expérience d’affichage multiple de la SPA.

1. Dans un nouvel onglet, ouvrez l’API de modèle JSON fournie par AEM : [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Il peut s’avérer utile d’utiliser une extension de navigateur pour [formater le JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   Ce contenu JSON est demandé lors du premier chargement de la SPA. La structure extérieure ressemble à ce qui suit :

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {},
      "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
      }
   }
   ```

   Sous `:children`, une entrée doit s’afficher pour chacune des pages créées. Le contenu de toutes les pages figure dans cette requête JSON initiale. Avec le routage de navigation, les vues suivantes du SPA seront rapidement chargées, puisque le contenu est déjà disponible côté client.

   Il n’est pas judicieux de charger **ALL** du contenu d’un SPA dans la requête JSON initiale, car cela ralentirait le chargement initial de la page. Examinons ensuite la manière dont la profondeur de hiérarchie des pages est collectée.

1. Accédez au modèle **SPA Racine** à l’adresse : [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Cliquez sur le **menu Propriétés de la page** > **Stratégie de page** :

   ![Ouvrir la stratégie de page pour SPA racine](assets/navigation-routing/open-page-policy.png)

1. Le modèle **SPA racine** comporte un onglet **Structure hiérarchique** supplémentaire pour contrôler le contenu JSON collecté. La **Profondeur de structure** détermine la profondeur dans la hiérarchie du site pour collecter les pages enfants sous la **racine**. Vous pouvez également utiliser le champ **Modèles de structure** pour filtrer les pages supplémentaires en fonction d’une expression régulière.

   Mettez à jour la **profondeur de structure** vers **2** :

   ![Profondeur de la structure de mise à jour](assets/navigation-routing/update-structure-depth.png)

   Cliquez sur **Terminé** pour enregistrer les modifications apportées à la stratégie.

1. Ouvrez à nouveau le modèle JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {}
      }
   }
   ```

   Notez que le chemin **Page 3** a été supprimé : `/content/wknd-spa-react/us/en/home/page-2/page-3` du modèle JSON initial. En effet, **La page 3** se trouve à un niveau 3 dans la hiérarchie et nous avons mis à jour la stratégie afin de n’inclure que le contenu à une profondeur maximale de niveau 2.

1. rouvrez la page d’accueil SPA : [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) et ouvrez les outils de développement de votre navigateur.

   Actualisez la page et la requête XHR doit s’afficher sur `/content/wknd-spa-react/us/en.model.json`, qui est la racine SPA. Notez que seules trois pages enfants sont incluses en fonction de la configuration de la profondeur de hiérarchie du modèle racine SPA effectué plus tôt dans le tutoriel. Cela n’inclut pas **Page 3**.

   ![Requête JSON initiale - SPA racine](assets/navigation-routing/initial-json-request.png)

1. Les outils de développement étant ouverts, utilisez le composant `Navigation` pour accéder directement à la **page 3** :

   Notez qu’une nouvelle requête XHR est envoyée à : `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Page trois - Requête XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager comprend que le contenu JSON **Page 3** n’est pas disponible et déclenche automatiquement la requête XHR supplémentaire.

1. Testez les liens profonds en accédant directement à : [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Notez également que le bouton Précédent du navigateur continue de fonctionner.

## Routage React Inspect  {#react-routing}

La navigation et le routage sont implémentés avec [React Router](https://reactrouter.com/). React Router est un ensemble de composants de navigation pour les applications React. [AEM les ](https://github.com/adobe/aem-react-core-wcm-components-base) composants principaux React utilisent les fonctionnalités de React Router pour implémenter le composant  **** Navigation utilisé dans les étapes précédentes.

Ensuite, examinez comment React Router est intégré à la SPA et testez-le à l’aide du composant [Lien](https://reactrouter.com/web/api/Link) de React Router.

1. Dans l’IDE, ouvrez le fichier `index.js` à l’adresse `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   Notez que le `App` est encapsulé dans le composant `Router` à partir de [Routeur React](https://reacttraining.com/react-router/). La balise `ModelManager`, fournie par le SDK JS de l’SPA éditeur d’AEM, ajoute les itinéraires dynamiques aux pages d’AEM en fonction de l’API du modèle JSON.

1. Ouvrez le fichier `Page.js` dans `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   Le composant `Page` SPA utilise la fonction `MapTo` pour mapper les **pages** dans AEM à un composant de SPA correspondant. L’utilitaire `withRoute` permet d’acheminer dynamiquement le SPA vers la page enfant AEM appropriée en fonction de la propriété `cqPath`.

1. Ouvrez le composant `Header.js` à l’adresse `ui.frontend/src/components/Header/Header.js`.
1. Mettez à jour `Header` pour encapsuler la balise `<h1>` dans un [lien](https://reactrouter.com/web/api/Link) vers la page d’accueil :

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   Au lieu d’utiliser une balise d’ancrage `<a>` par défaut, nous utilisons `<Link>` fournie par React Router. Tant que `to=` pointe vers un itinéraire valide, le SPA passe à cet itinéraire et **non** effectue une actualisation complète de la page. Nous codons ici en dur le lien vers la page d’accueil pour illustrer l’utilisation de `Link`.

1. Mettez à jour le test à `App.test.js` à `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Puisque nous utilisons des fonctionnalités du routeur React dans un composant statique référencé dans `App.js`, nous devons mettre à jour le test unitaire pour en tenir compte.

1. Ouvrez un terminal, accédez à la racine du projet et déployez le projet pour AEM à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Accédez à l’une des pages du SPA dans AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   Au lieu d’utiliser le composant `Navigation` pour naviguer, utilisez le lien dans la balise `Header`.

   ![Lien d’en-tête](assets/navigation-routing/header-link.png)

   Observez qu’une actualisation de page complète est **non** déclenchée et que le routage SPA fonctionne.

1. Si vous le souhaitez, vous pouvez tester le fichier `Header.js` à l’aide d’une balise d’ancrage `<a>` standard :

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   Cela peut aider à illustrer la différence entre le routage SPA et les liens de page web standard.

## Félicitations ! {#congratulations}

Félicitations, vous avez appris comment plusieurs vues dans la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de formulaires. La navigation dynamique a été mise en oeuvre à l’aide de React Router et ajoutée au composant `Header`.
