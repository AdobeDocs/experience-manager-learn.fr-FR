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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# Ajout de la navigation et du routage {#navigation-routing}

Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de la version de la suite de rapports. La navigation dynamique est mise en oeuvre à l’aide des composants principaux React Router et React.

## Objectif

1. Découvrez les options de routage du modèle SPA disponibles lors de l’utilisation de SPA Editor.
1. Découvrez comment utiliser [Routeur React](https://reacttraining.com/react-router/) pour naviguer entre les différentes vues de la SPA.
1. Utilisez AEM composants principaux React pour mettre en oeuvre une navigation dynamique pilotée par la hiérarchie de pages AEM.

## Ce que vous allez créer

Ce chapitre ajoute une navigation à un SPA dans AEM. Le menu de navigation est piloté par la hiérarchie AEM page et utilise le modèle JSON fourni par la variable [Composant principal de navigation](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![Navigation ajoutée](assets/navigation-routing/navigation-added.png)

## Prérequis

Examinez les outils et les instructions requis pour configurer une [environnement de développement local](overview.md#local-dev-environment). Ce chapitre est la suite du chapitre [Mappage des composants](map-components.md) , toutefois, pour suivre l’exemple, vous avez besoin d’un projet AEM activé SPA déployé sur une instance d’AEM locale.

## Ajouter la navigation au modèle {#add-navigation-template}

1. Ouvrez un navigateur et connectez-vous à AEM, [http://localhost:4502/](http://localhost:4502/). La base de code de départ doit déjà être déployée.
1. Accédez au **Modèle de page SPA**: [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Sélectionnez l’élément le plus externe **Conteneur de mise en page racine** et cliquez sur son **Stratégie** icône . Soyez prudent **not** pour sélectionner la variable **Conteneur de mises en page** déverrouillé pour la création.

   ![Icône de stratégie de conteneur de mises en page racine](assets/navigation-routing/root-layout-container-policy.png)

1. Création d’une stratégie nommée **Structure SPA**:

   ![Stratégie de structure SPA](assets/navigation-routing/spa-policy-update.png)

   Sous **Composants autorisés** > **Général** > sélectionnez l’option **Conteneur de mises en page** composant.

   Sous **Composants autorisés** > **WKND SPA REACT - STRUCTURE** > sélectionnez l’option **Navigation** component :

   ![Sélectionner le composant Navigation](assets/navigation-routing/select-navigation-component.png)

   Sous **Composants autorisés** > **WKND SPA REACT - Contenu** > sélectionnez l’option **Image** et **Texte** composants. Vous devez avoir 4 composants au total sélectionnés.

   Cliquez sur **Terminé** pour enregistrer les modifications.

1. Actualisez la page, puis ajoutez le **Navigation** au-dessus du composant déverrouillé **Conteneur de mises en page**:

   ![ajouter le composant Navigation au modèle](assets/navigation-routing/add-navigation-component.png)

1. Sélectionnez la **Navigation** composant et cliquez sur son **Stratégie** pour modifier la stratégie.
1. Création d’une stratégie avec une **Titre de la stratégie** de **Navigation SPA**.

   Sous , **Propriétés**:

   * Définissez la variable **Racine de navigation** to `/content/wknd-spa-react/us/en`.
   * Définissez la variable **Exclure les niveaux racine** to **1**.
   * Décocher **Collecter toutes les pages enfants**.
   * Définissez la variable **Profondeur de la structure de navigation** to **3**.

   ![Configuration de la stratégie de navigation](assets/navigation-routing/navigation-policy.png)

   Cela collectera la navigation à 2 niveaux en dessous de `/content/wknd-spa-react/us/en`.

1. Après avoir enregistré vos modifications, vous devriez voir la valeur renseignée `Navigation` dans le modèle :

   ![Composant de navigation renseigné](assets/navigation-routing/populated-navigation.png)

## Créer des pages enfants

Créez ensuite des pages supplémentaires dans AEM qui serviront de vues différentes dans le SPA. Nous examinerons également la structure hiérarchique du modèle JSON fourni par AEM.

1. Accédez au **Sites** console : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Sélectionnez la **Page d’accueil WKND SPA React** et cliquez sur **Créer** > **Page**:

   ![Créer une page](assets/navigation-routing/create-new-page.png)

1. Sous **Modèle** select **Page SPA**. Sous **Propriétés** enter **Page 1** pour le **Titre** et **page-1** comme nom.

   ![Renseignez les propriétés initiales de la page](assets/navigation-routing/initial-page-properties.png)

   Cliquez sur **Créer** et dans la fenêtre contextuelle de la boîte de dialogue, cliquez sur **Ouvrir** pour ouvrir la page dans AEM SPA Editor.

1. Ajouter un nouveau **Texte** au composant principal **Conteneur de mises en page**. Modifiez le composant et saisissez le texte : **Page 1** à l’aide de l’éditeur de texte enrichi et de la variable **H2** élément .

   ![Exemple de contenu page 1](assets/navigation-routing/page-1-sample-content.png)

   N’hésitez pas à ajouter du contenu supplémentaire, comme une image.

1. Revenez à la console AEM Sites et répétez les étapes ci-dessus, en créant une seconde page nommée **Page 2** comme frère de **Page 1**.
1. Enfin, créez une troisième page, **Page 3** mais en tant que **child** de **Page 2**. Une fois la hiérarchie du site terminée, elle doit se présenter comme suit :

   ![Exemple de hiérarchie de site](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Le composant Navigation peut désormais être utilisé pour accéder à différentes zones du SPA.

   ![Navigation et routage](assets/navigation-routing/navigation-working.gif)

1. Ouvrez la page en dehors de l’éditeur d’AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). Utilisez la variable **Navigation** pour accéder à différentes vues de l’application.

1. Utilisez les outils de développement de votre navigateur pour examiner les requêtes réseau au fur et à mesure de votre navigation. Les captures d’écran ci-dessous sont capturées à partir du navigateur Google Chrome.

   ![Observez les requêtes réseau](assets/navigation-routing/inspect-network-requests.png)

   Notez qu’après le chargement initial de la page, la navigation suivante n’entraîne pas une actualisation complète de la page et que le trafic réseau est réduit lors du retour aux pages précédemment visitées.

## Modèle JSON de page de hiérarchie {#hierarchy-page-json-model}

Examinez ensuite le modèle JSON qui anime l’expérience d’affichage multiple de la SPA.

1. Dans un nouvel onglet, ouvrez l’API de modèle JSON fournie par AEM : [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Il peut s’avérer utile d’utiliser une extension de navigateur pour [formater le fichier JSON ;](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

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

   Sous `:children` vous devriez voir une entrée pour chacune des pages créées. Le contenu de toutes les pages figure dans cette requête JSON initiale. Avec le routage de navigation, les vues suivantes du SPA sont chargées rapidement, puisque le contenu est déjà disponible côté client.

   Il n’est pas judicieux de charger **TOUT** du contenu d’un SPA dans la requête JSON initiale, car cela ralentirait le chargement initial de la page. Examinons ensuite la manière dont la profondeur de hiérarchie des pages est collectée.

1. Accédez au **Racine SPA** à l’adresse : [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Cliquez sur le bouton **Menu Propriétés de la page** > **Stratégie de page**:

   ![Ouvrir la stratégie de page pour SPA racine](assets/navigation-routing/open-page-policy.png)

1. Le **Racine SPA** modèle comporte un élément supplémentaire **Structure hiérarchique** pour contrôler le contenu JSON collecté. Le **Profondeur de structure** détermine la profondeur de la hiérarchie du site pour collecter les pages enfants sous la propriété **root**. Vous pouvez également utiliser la variable **Modèles de structure** pour filtrer les pages supplémentaires en fonction d’une expression régulière.

   Mettez à jour le **Profondeur de structure** to **2**:

   ![Profondeur de la structure de mise à jour](assets/navigation-routing/update-structure-depth.png)

   Cliquez sur **Terminé** pour enregistrer les modifications apportées à la stratégie.

1. Réouverture du modèle JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

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

   Notez que la variable **Page 3** path a été supprimé : `/content/wknd-spa-react/us/en/home/page-2/page-3` du modèle JSON initial. Ceci est dû au fait que **Page 3** se trouve à un niveau 3 dans la hiérarchie et nous avons mis à jour la stratégie afin d’inclure uniquement le contenu à une profondeur maximale de niveau 2.

1. rouvrez la page d’accueil SPA : [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) et ouvrez les outils de développement de votre navigateur.

   Actualisez la page et la requête XHR doit s’afficher pour `/content/wknd-spa-react/us/en.model.json`, qui est la racine SPA. Notez que seules trois pages enfants sont incluses en fonction de la configuration de la profondeur de hiérarchie du modèle racine SPA effectué plus tôt dans le tutoriel. Cela n’inclut pas **Page 3**.

   ![Requête JSON initiale - SPA racine](assets/navigation-routing/initial-json-request.png)

1. Lorsque les outils de développement sont ouverts, utilisez la méthode `Navigation` pour accéder directement à **Page 3**:

   Notez qu’une nouvelle requête XHR est envoyée à : `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Page trois - Requête XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager comprend que la fonction **Page 3** Le contenu JSON n’est pas disponible et déclenche automatiquement la requête XHR supplémentaire.

1. Testez les liens profonds en accédant directement à : [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Notez également que le bouton Précédent du navigateur continue de fonctionner.

## Routage React Inspect  {#react-routing}

La navigation et le routage sont implémentés avec [Routeur React](https://reactrouter.com/). React Router est un ensemble de composants de navigation pour les applications React. [Composants principaux React AEM](https://github.com/adobe/aem-react-core-wcm-components-base) utilise les fonctionnalités de React Router pour mettre en oeuvre la variable **Navigation** du composant utilisé dans les étapes précédentes.

Ensuite, examinez la manière dont React Router est intégré à la SPA et testez-la à l’aide de la fonction [Lien](https://reactrouter.com/web/api/Link) composant.

1. Dans l’IDE, ouvrez le fichier . `index.js` at `ui.frontend/src/index.js`.

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

   Notez que la variable `App` est encapsulé dans la variable `Router` du composant [Routeur React](https://reacttraining.com/react-router/). Le `ModelManager`, fourni par le SDK JS de l’SPA d’AEM, ajoute les itinéraires dynamiques aux pages d’AEM en fonction de l’API du modèle JSON.

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

   Le `Page` SPA composant utilise la variable `MapTo` fonction à mapper **Pages** dans AEM à un composant SPA correspondant. Le `withRoute` aide à acheminer dynamiquement le SPA vers la page enfant AEM appropriée en fonction de la variable `cqPath` .

1. Ouvrez le `Header.js` component at `ui.frontend/src/components/Header/Header.js`.
1. Mettez à jour le `Header` pour encapsuler la variable `<h1>` dans une balise [Lien](https://reactrouter.com/web/api/Link) sur la page d’accueil :

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

   Au lieu d’utiliser une valeur par défaut `<a>` balise d’ancrage que nous utilisons `<Link>` fourni par React Router. Tant que la variable `to=` pointe vers un itinéraire valide, SPA passera à cet itinéraire et **not** effectuez une actualisation complète de la page. Nous allons simplement coder en dur le lien vers la page d’accueil pour illustrer l’utilisation de la fonction `Link`.

1. Mettez à jour le test à l’adresse `App.test.js` at `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   Puisque nous utilisons les fonctionnalités de React Router dans un composant statique référencé dans `App.js` nous devons mettre à jour le test unitaire pour en tenir compte.

1. Ouvrez un terminal, accédez à la racine du projet et déployez le projet pour AEM à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Accédez à l’une des pages du SPA dans AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   Au lieu d’utiliser la variable `Navigation` pour naviguer, utilisez le lien dans la variable `Header`.

   ![Lien d’en-tête](assets/navigation-routing/header-link.png)

   Notez qu’une actualisation complète de la page est **not** déclenché et que le routage SPA fonctionne.

1. Si vous le souhaitez, vous pouvez tester la variable `Header.js` à l’aide d’un fichier standard `<a>` balise d’ancrage :

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   Cela peut aider à illustrer la différence entre le routage SPA et les liens de page web standard.

## Félicitations ! {#congratulations}

Félicitations, vous avez appris comment plusieurs vues dans la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de formulaires. La navigation dynamique a été mise en oeuvre à l’aide de React Router et ajoutée au `Header` composant.
