---
title: Ajouter la navigation et le routage | Prise en main de l'AEM SPA Editor et réaction
description: Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant des pages AEM avec le SDK de l’éditeur SPA. La navigation dynamique est mise en oeuvre à l'aide du Routeur de réaction et ajoutée à un composant d'en-tête existant.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '2112'
ht-degree: 2%

---


# Ajouter la navigation et le routage {#navigation-routing}

Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant des pages AEM avec le SDK de l’éditeur SPA. La navigation dynamique est mise en oeuvre à l&#39;aide du Routeur de réaction et ajoutée à un composant d&#39;en-tête existant.

## Intention

1. Découvrez les options de routage de modèle SPA disponibles lors de l’utilisation de l’éditeur de SPA.
1. Apprenez à utiliser [Réagir au routeur](https://reacttraining.com/react-router/) pour naviguer entre les différentes vues du SPA.
1. Implémentez une navigation dynamique pilotée par la hiérarchie des pages AEM.

## Ce que vous allez créer

Ce chapitre ajoute un menu de navigation à un composant `Header` existant. Le menu de navigation sera piloté par la hiérarchie des pages AEM et utilisera le modèle JSON fourni par le [composant principal de navigation](https://docs.adobe.com/content/help/fr/experience-manager-core-components/using/components/navigation.html).

![Navigation mise en oeuvre](assets/navigation-routing/final-navigation-implemented.gif)

## Conditions préalables

Examinez les outils et les instructions nécessaires pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtention du code

1. Téléchargez le point de départ de ce didacticiel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Déployez la base de code sur une instance AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installez le package fini pour le site de référence traditionnel [WKND](https://github.com/adobe/aem-guides-wknd/releases/latest). Les images fournies par [le site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) seront réutilisées sur le SPA WKND. Le package peut être installé à l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou vérifier le code localement en passant à la branche `React/navigation-routing-solution`.

## Mises à jour de l’en-tête Inspect {#inspect-header}

Dans les chapitres précédents, le composant `Header` a été ajouté en tant que composant React pur inclus via `App.js`. Dans ce chapitre, le composant `Header` a été supprimé et sera ajouté via l&#39;[éditeur de modèles](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Cela permet aux utilisateurs de configurer le menu de navigation de `Header` depuis l&#39;AEM.

>[!NOTE]
>
> Plusieurs mises à jour CSS et JavaScript ont déjà été apportées à la base de code pour début ce chapitre. Pour vous concentrer sur les concepts de base, et non **tous** des changements de code sont abordés. Vous pouvez vue les modifications complètes [ici](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. Dans l&#39;IDE de votre choix, ouvrez le projet de démarrage SPA pour ce chapitre.
1. Sous le module `ui.frontend`, inspectez le fichier `Header.js` à l&#39;adresse : `ui.frontend/src/components/Header/Header.js`.

   Plusieurs mises à jour ont été effectuées, notamment l&#39;ajout d&#39;un `HeaderEditConfig` et d&#39;un `MapTo` pour permettre le mappage du composant à un composant AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Dans le module `ui.apps`, inspectez la définition du composant de l&#39;AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   Le composant `Header` de l&#39;AEM hérite de toutes les fonctionnalités du [composant principal de navigation](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) par l&#39;intermédiaire de la propriété `sling:resourceSuperType`.

## Ajouter l&#39;en-tête au modèle {#add-header-template}

1. Ouvrez un navigateur et connectez-vous à AEM, [http://localhost:4502/](http://localhost:4502/). La base de code de démarrage doit déjà être déployée.
1. Accédez au **modèle de page SPA** : [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Sélectionnez le **Conteneur de mise en page racine le plus à l&#39;extérieur** et cliquez sur son icône **Stratégie**. Veillez à **ne pas** sélectionner le **Conteneur de mise en page** déverrouillé pour la création.

   ![Sélectionner l&#39;icône de stratégie de conteneur de disposition racine](assets/navigation-routing/root-layout-container-policy.png)

1. Créez une nouvelle stratégie nommée **SPA Structure** :

   ![Stratégie de structure SPA](assets/navigation-routing/spa-policy-update.png)

   Sous **Composants autorisés** > **Général** > sélectionnez le composant **Conteneur de mise en page**.

   Sous **Composants autorisés** > **REACT SPA WKND - STRUCTURE** > sélectionnez le composant **En-tête** :

   ![Sélectionner le composant d’en-tête](assets/navigation-routing/select-header-component.png)

   Sous **Composants autorisés** > **WKND SPA REACT - Content** > sélectionnez les composants **Image** et **Texte**. Vous devez sélectionner 4 composants au total.

   Cliquez sur **Terminé** pour enregistrer les modifications.

1. Actualisez la page et ajoutez le composant **En-tête** au-dessus du Conteneur de mise en page **non verrouillé** :

   ![ajouter un composant d’en-tête au modèle](./assets/navigation-routing/add-header-component.gif)

1. Sélectionnez le composant **En-tête** et cliquez sur son icône **Stratégie** pour modifier la stratégie.
1. Créez une nouvelle stratégie avec un **Titre de la stratégie** de **En-tête SPA WKND**.

   Sous **Propriétés** :

   * Définissez **Racine de navigation** sur `/content/wknd-spa-react/us/en`.
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Décochez **Collecter toutes les pages enfants**.
   * Définissez **Profondeur de structure de navigation** sur **3**.

   ![Configurer la stratégie d&#39;en-tête](assets/navigation-routing/header-policy.png)

   Cette opération collectera les 2 niveaux de navigation en dessous de `/content/wknd-spa-react/us/en`.

1. Après avoir enregistré vos modifications, vous devriez voir le `Header` renseigné dans le modèle :

   ![Composant d’en-tête renseigné](assets/navigation-routing/populated-header.png)

## Créer des pages enfants

Créez ensuite des pages supplémentaires dans AEM qui serviront de vues différentes dans le SPA. Nous examinerons également la structure hiérarchique du modèle JSON fourni par AEM.

1. Accédez à la console **Sites** : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Sélectionnez la **Page d&#39;accueil de réaction SPA WKND** et cliquez sur **Créer** > **Page** :

   ![Créer une page](assets/navigation-routing/create-new-page.png)

1. Sous **Modèle** sélectionnez **SPA Page**. Sous **Propriétés**, entrez **Page 1** pour le **titre** et **page-1** comme nom.

   ![Saisissez les propriétés de la page initiale](assets/navigation-routing/initial-page-properties.png)

   Cliquez sur **Créer** et dans la fenêtre contextuelle de la boîte de dialogue, cliquez sur **Ouvrir** pour ouvrir la page dans l’AEM SPA Editor.

1. Ajoutez un nouveau composant **Texte** au Conteneur principal **Disposition**. Modifiez le composant et saisissez le texte : **Page 1** à l’aide de l’ETC et de l’élément **H1** (vous devrez passer en mode plein écran pour modifier les éléments de paragraphe)

   ![Exemple de page de contenu 1](assets/navigation-routing/page-1-sample-content.png)

   N’hésitez pas à ajouter du contenu supplémentaire, tel qu’une image.

1. Revenez à la console AEM Sites et répétez les étapes ci-dessus, en créant une deuxième page nommée **Page 2** en tant que frère de **Page 1**.
1. Enfin, créez une troisième page, **Page 3** mais en tant qu&#39;**enfant** de **Page 2**. Une fois la hiérarchie du site terminée, elle doit se présenter comme suit :

   ![Exemple de hiérarchie de site](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Dans un nouvel onglet, ouvrez l’API du modèle JSON fournie par AEM : [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Ce contenu JSON est demandé lors du premier chargement du SPA. La structure extérieure ressemble à ce qui suit :

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

   Sous `:children`, vous devriez voir une entrée pour chacune des pages créées. Le contenu de toutes les pages figure dans cette requête JSON initiale. Une fois le routage de navigation mis en oeuvre, les vues suivantes du SPA seront rapidement chargées, puisque le contenu est déjà disponible côté client.

   Il n’est pas judicieux de charger **ALL** du contenu d’une SPA dans la demande JSON initiale, car cela ralentirait le chargement initial de la page. Examinons ensuite comment la profondeur d’hiérarchie des pages est collectée.

1. Accédez au modèle **SPA racine** à l&#39;adresse : [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   Cliquez sur le menu **Propriétés de la page** > **Stratégie de page** :

   ![Ouvrir la stratégie de page pour SPA racine](assets/navigation-routing/open-page-policy.png)

1. Le modèle **SPA racine** comporte un onglet **Structure hiérarchique** supplémentaire pour contrôler le contenu JSON collecté. **Profondeur de structure** détermine la profondeur dans la hiérarchie du site pour collecter les pages enfants sous la **racine**. Vous pouvez également utiliser le champ **Modèles de structure** pour filtrer d’autres pages en fonction d’une expression régulière.

   Mettez à jour **Profondeur de structure** en **2** :

   ![Mettre à jour la profondeur de structure](assets/navigation-routing/update-structure-depth.png)

   Cliquez sur **Terminé** pour enregistrer les modifications apportées à la stratégie.

1. Ouvrez de nouveau le modèle JSON [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

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

   Notez que le chemin d&#39;accès **Page 3** a été supprimé : `/content/wknd-spa-react/us/en/home/page-2/page-3` du modèle JSON initial.

   Par la suite, nous verrons comment le AEM SPA Editor SDK peut charger dynamiquement du contenu supplémentaire.

## Mise en oeuvre de la navigation

Ensuite, implémentez le menu de navigation dans le cadre de `Header`. Nous pourrions ajouter directement le code dans `Header.js`, mais une meilleure pratique consiste à éviter les composants volumineux. Nous allons plutôt mettre en oeuvre un composant SPA `Navigation` qui pourrait être réutilisé ultérieurement.

1. Consultez le fichier JSON exposé par le composant `Header` de l’AEM à l’adresse [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) :

   ```json
   ...
   "header": {
       "items": [
       {
       "level": 0,
       "active": true,
       "path": "/content/wknd-spa-react/us/en/home",
       "description": null,
       "url": "/content/wknd-spa-react/us/en/home.html",
       "lastModified": 1589062597083,
       "title": "WKND SPA React Home Page",
       "children": [
               {
               "children": [],
               "level": 1,
               "active": false,
               "path": "/content/wknd-spa-react/us/en/home/page-1",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-1.html",
               "lastModified": 1589429385100,
               "title": "Page 1"
               },
               {
               "level": 1,
               "active": true,
               "path": "/content/wknd-spa-react/us/en/home/page-2",
               "description": null,
               "url": "/content/wknd-spa-react/us/en/home/page-2.html",
               "lastModified": 1589429603507,
               "title": "Page 2",
               "children": [
                   {
                   "children": [],
                   "level": 2,
                   "active": false,
                   "path": "/content/wknd-spa-react/us/en/home/page-2/page-3",
                   "description": null,
                   "url": "/content/wknd-spa-react/us/en/home/page-2/page-3.html",
                   "lastModified": 1589430413831,
                   "title": "Page 3"
                   }
               ],
               }
           ]
           }
       ],
   ":type": "wknd-spa-react/components/header"
   ```

   La nature hiérarchique des pages AEM est modélisée dans le fichier JSON qui peut être utilisé pour remplir un menu de navigation. Rappelez-vous que le composant `Header` hérite de toutes les fonctionnalités du [Composant principal de navigation](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) et que le contenu exposé via le JSON est automatiquement mappé aux props Réagir.

1. Ouvrez une nouvelle fenêtre de terminal et accédez au dossier `ui.frontend` du projet SPA. Début le **webpack-dev-server** avec la commande `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Ouvrez un nouvel onglet de navigateur et accédez à [http://localhost:3000/](http://localhost:3000/).

   **webpack-dev-server** doit être configuré pour proxy du modèle JSON à partir d’une instance locale d’AEM (`ui.frontend/.env.development`). Cela nous permettra de coder directement en fonction du contenu créé dans AEM exercice précédent. Assurez-vous d’être authentifié dans AEM au cours de la même session de navigation.

   ![activation/désactivation du menu](./assets/navigation-routing/nav-toggle-static.gif)

   La fonction de basculement de menu `Header` est actuellement mise en oeuvre. Ensuite, mettez en oeuvre le menu de navigation.

1. Revenez à l&#39;IDE de votre choix et ouvrez `Header.js` à `ui.frontend/src/components/Header/Header.js`.
1. Mettez à jour la méthode `homeLink()` pour supprimer la chaîne codée en dur et utilisez les props dynamiques transmises par le composant AEM :

   ```js
   /* Header.js */
   ...
   get homeLink() {
        //expect a single root defined as part of the navigation
       if(!this.props.items || this.props.items.length !== 1) {
           return null;
       }
   
       return this.props.items[0].url;
   }
   ...
   ```

   Le code ci-dessus renseigne une url en fonction de l’élément de navigation racine configuré par le composant. `homeLink()` est utilisée pour remplir le logo dans la  `logo()` méthode et pour déterminer si le bouton Précédent doit être affiché dans  `backButton()`.

   Enregistrez les modifications dans `Header.js`.

1. Ajoutez une ligne en haut de `Header.js` pour importer le composant `Navigation` sous les autres importations :

   ```js
   /* Header.js */
   ...
   import Navigation from '../Navigation/Navigation';
   ```

1. Mettez ensuite à jour la méthode `get navigation()` pour instancier le composant `Navigation` :

   ```js
   /* Header.js */
   ...
   get navigation() {
       //pass all the props to Navigation component
       return <Navigation {...this.props} />;
   }
   ...
   ```

   Comme nous l&#39;avons mentionné plus haut, au lieu d&#39;implémenter la navigation dans `Header`, nous implémenterons la majorité de la logique dans le composant `Navigation`.  Les props de `Header` incluent la structure JSON nécessaire pour construire le menu, nous transmettons toutes les props.
1. Ouvrez le fichier `Navigation.js` dans `ui.frontend/src/components/Navigation/Navigation.js`.
1. Implémentez la méthode `renderGroupNav(children)` :

   ```js
   /* Navigation.js */
   ...
   renderGroupNav(children) {
   
       if(children === null || children.length < 1 ) {
           return null;
       }
       return (<ul className={this.baseCss + '__group'}>
                   {children.map(
                       (item,index) => { return this.renderNavItem(item,index)}
                   )}
               </ul>
       );
   }
   ...
   ```

   Cette méthode utilise un tableau d’éléments de navigation, `children`, et crée une liste non ordonnée. Il effectue ensuite une itération sur la baie et transmet l&#39;élément à `renderNavItem`, qui sera ensuite implémenté.

1. Mettez en oeuvre le `renderNavItem` :

   ```js
   /* Navigation.js */
   ...
   renderNavItem(item, index) {
       const cssClass = this.baseCss + '__item ' + 
                        this.baseCss + '__item--level-' + item.level + ' ' +
                        (item.active ? ' ' + this.baseCss + '__item--active' : '');
       return (
           <li key={this.baseCss + '__item-' + index} className={cssClass}>
                   { this.renderLink(item) }
                   { this.renderGroupNav(item.children) }
           </li>
       );
   }
   ...
   ```

   Cette méthode effectue le rendu d’un élément de liste, avec des classes CSS basées sur les propriétés `level` et `active`. La méthode appelle ensuite `renderLink` pour créer la balise d’ancrage. Le contenu `Navigation` étant hiérarchique, une stratégie récursive est utilisée pour appeler `renderGroupNav` pour les enfants de l’élément actuel.

1. Implémentez la méthode `renderLink` :

   Ajoutez une méthode d&#39;importation pour le composant [Link](https://reacttraining.com/react-router/web/api/Link), qui fait partie du routeur React, en haut du fichier avec les autres importations :

   ```js
   import {Link} from "react-router-dom";
   ```

   Terminez ensuite l&#39;implémentation de la méthode `renderLink` :

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Notez que, au lieu d’une balise d’ancrage normale, `<a>`, le composant [Link](https://reacttraining.com/react-router/web/api/Link) est utilisé. Cela permet de s’assurer que l’actualisation de la page complète n’est pas déclenchée et qu’elle exploite au contraire le routeur React fourni par le AEM SPA Editor JS SDK.

1. Enregistrez les modifications dans `Navigation.js` et revenez à **webpack-dev-server** : [http://localhost:3000](http://localhost:3000)

   ![Navigation dans l’en-tête terminée](assets/navigation-routing/completed-header.png)

   Ouvrez la navigation en cliquant sur la bascule du menu et vous devriez voir les liens de navigation renseignés. Vous devriez être en mesure de naviguer vers différentes vues du SPA.

## Inspect, le Routage SPA

Maintenant que la navigation a été mise en oeuvre, inspectez le routage en AEM.

1. Dans l&#39;IDE, ouvrez le fichier `index.js` à `ui.frontend/src/index.js`.

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

   Remarquez que le `App` est encapsulé dans le composant `Router` de [Routeur de réaction](https://reacttraining.com/react-router/). Le `ModelManager`, fourni par le AEM SPA Editor JS SDK, ajoute les itinéraires dynamiques aux pages de l’AEM en fonction de l’API du modèle JSON.

1. Ouvrez un terminal, accédez à la racine du projet et déployez le projet pour AEM en utilisant vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Accédez à la page d&#39;accueil SPA dans AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) et ouvrez les outils de développement de votre navigateur. Les captures d’écran ci-dessous sont extraites du navigateur Google Chrome.

   Actualisez la page et vous devriez voir une requête XHR à `/content/wknd-spa-react/us/en.model.json`, qui est la racine SPA. Notez que seules trois pages enfants sont incluses en fonction de la configuration de la profondeur de hiérarchie du modèle racine SPA effectué plus tôt dans le didacticiel. Cela n&#39;inclut pas **Page 3**.

   ![Demande JSON initiale - SPA racine](assets/navigation-routing/initial-json-request.png)

1. Les outils de développement étant ouverts, utilisez la navigation `Header` pour accéder à **Page 3** :

   ![Page 3 Navigation](assets/navigation-routing/page-three-navigation.png)

   Notez qu’une nouvelle demande XHR est envoyée à : `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Page 3 Demande XHR](assets/navigation-routing/page-3-xhr-request.png)

   Le Gestionnaire de modèles d&#39;AEM sait que le contenu JSON **Page 3** n&#39;est pas disponible et déclenche automatiquement la requête XHR supplémentaire.

1. Continuez à naviguer dans le SPA en utilisant les différents liens de navigation du composant `Header`. Observez qu’aucune requête XHR supplémentaire n’est effectuée et qu’aucune actualisation de page complète n’est effectuée. Cela permet à l’utilisateur final de SPA plus rapidement et de réduire les demandes inutiles à l’AEM.

   ![Navigation mise en oeuvre](assets/navigation-routing/final-navigation-implemented.gif)

1. Testez les liens profonds en naviguant directement vers : [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observez que le bouton Retour du navigateur continue de fonctionner.

## Félicitations! {#congratulations}

Félicitations, vous avez appris comment plusieurs vues du SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de SPA. La navigation dynamique a été mise en oeuvre à l&#39;aide du Routeur de réaction et ajoutée au composant `Header`.

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou vérifier le code localement en passant à la branche `React/navigation-routing-solution`.
