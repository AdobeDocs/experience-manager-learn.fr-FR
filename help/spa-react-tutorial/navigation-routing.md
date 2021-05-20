---
title: Ajout de la navigation et du routage | Prise en main de l’éditeur SPA d’AEM et de React
description: Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de la version de la suite de rapports. La navigation dynamique est mise en oeuvre à l’aide de React Router et ajoutée à un composant d’en-tête existant.
sub-product: sites
feature: Éditeur de SPA
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2115'
ht-degree: 2%

---


# Ajout de la navigation et du routage {#navigation-routing}

Découvrez comment plusieurs vues de la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de la version de la suite de rapports. La navigation dynamique est mise en oeuvre à l’aide de React Router et ajoutée à un composant d’en-tête existant.

## Intention

1. Découvrez les options de routage du modèle SPA disponibles lors de l’utilisation de SPA Editor.
1. Découvrez comment utiliser [le routeur de réaction](https://reacttraining.com/react-router/) pour naviguer entre les différentes vues de la SPA.
1. Mettez en oeuvre une navigation dynamique pilotée par la hiérarchie de pages AEM.

## Ce que vous allez créer

Ce chapitre ajoute un menu de navigation à un composant `Header` existant. Le menu de navigation sera piloté par la hiérarchie AEM page et utilisera le modèle JSON fourni par le [composant principal de navigation](https://docs.adobe.com/content/help/fr/experience-manager-core-components/using/components/navigation.html).

![Navigation implémentée](assets/navigation-routing/final-navigation-implemented.gif)

## Prérequis

Examinez les outils et instructions requis pour configurer un [environnement de développement local](overview.md#local-dev-environment).

### Obtention du code

1. Téléchargez le point de départ de ce tutoriel via Git :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/navigation-routing-start
   ```

1. Déployez la base de code sur une instance d’AEM locale à l’aide de Maven :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   Si vous utilisez [AEM 6.x](overview.md#compatibility), ajoutez le profil `classic` :

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. Installez le package terminé pour le [site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) traditionnel. Les images fournies par [le site de référence WKND](https://github.com/adobe/aem-guides-wknd/releases/latest) seront réutilisées sur le SPA WKND. Le package peut être installé à l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou extraire le code localement en passant à la branche `React/navigation-routing-solution`.

## Mises à jour de l’en-tête Inspect {#inspect-header}

Dans les chapitres précédents, le composant `Header` a été ajouté en tant que composant React pur inclus via `App.js`. Dans ce chapitre, le composant `Header` a été supprimé et sera ajouté via l’[éditeur de modèles](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html). Cela permet aux utilisateurs de configurer le menu de navigation de `Header` dans AEM.

>[!NOTE]
>
> Plusieurs mises à jour CSS et JavaScript ont déjà été apportées à la base de code pour commencer ce chapitre. Pour se concentrer sur les concepts de base, et non **tous** des modifications de code sont abordés. Vous pouvez afficher toutes les modifications [ici](https://github.com/adobe/aem-guides-wknd-spa/compare/React/map-components-solution...React/navigation-routing-start).

1. Dans l’IDE de votre choix, ouvrez le projet SPA de démarrage pour ce chapitre.
1. Sous le module `ui.frontend` , inspectez le fichier `Header.js` à l’adresse : `ui.frontend/src/components/Header/Header.js`.

   Plusieurs mises à jour ont été effectuées, notamment l’ajout d’un `HeaderEditConfig` et d’un `MapTo` pour permettre le mappage du composant à un composant AEM `wknd-spa-react/components/header`.

   ```js
   /* Header.js */
   ...
   export const HeaderEditConfig = {
       ...
   }
   ...
   MapTo('wknd-spa-react/components/header')(withRouter(Header), HeaderEditConfig);
   ```

1. Dans le module `ui.apps` , examinez la définition du composant d’AEM `Header` : `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/header/.content.xml` :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Header"
       sling:resourceSuperType="wknd-spa-react/components/navigation"
       componentGroup="WKND SPA React - Structure"/>
   ```

   Le composant `Header` AEM héritera de toutes les fonctionnalités du [composant principal de navigation](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) via la propriété `sling:resourceSuperType`.

## Ajouter l’en-tête au modèle {#add-header-template}

1. Ouvrez un navigateur et connectez-vous à AEM, [http://localhost:4502/](http://localhost:4502/). La base de code de départ doit déjà être déployée.
1. Accédez au **Modèle de page SPA** : [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. Sélectionnez le **conteneur de mise en page racine le plus éloigné** et cliquez sur son icône **Stratégie**. Veillez à **ne pas** sélectionner le **conteneur de mises en page** déverrouillé pour la création.

   ![Icône de stratégie de conteneur de mises en page racine](assets/navigation-routing/root-layout-container-policy.png)

1. Créez une nouvelle stratégie nommée **SPA Structure** :

   ![Stratégie de structure SPA](assets/navigation-routing/spa-policy-update.png)

   Sous **Composants autorisés** > **Général** > sélectionnez le composant **Conteneur de mises en page** .

   Sous **Composants autorisés** > **WKND SPA REACT - STRUCTURE** > sélectionnez le composant **En-tête** :

   ![Sélectionner le composant d’en-tête](assets/navigation-routing/select-header-component.png)

   Sous **Composants autorisés** > **WKND SPA REACT - Content** > sélectionnez les composants **Image** et **Texte**. Vous devez avoir 4 composants au total sélectionnés.

   Cliquez sur **Terminé** pour enregistrer les modifications.

1. Actualisez la page et ajoutez le composant **En-tête** au-dessus du **Conteneur de mises en page déverrouillé** :

   ![ajouter le composant En-tête au modèle](./assets/navigation-routing/add-header-component.gif)

1. Sélectionnez le composant **En-tête** et cliquez sur son icône **Stratégie** pour modifier la stratégie.
1. Créez une nouvelle stratégie avec un **Titre de la stratégie** de **En-tête SPA WKND**.

   Sous **Propriétés** :

   * Définissez la **racine de navigation** sur `/content/wknd-spa-react/us/en`.
   * Définissez **Exclure les niveaux racine** sur **1**.
   * Décochez **Collecter toutes les pages enfants**.
   * Définissez la **Profondeur de la structure de navigation** sur **3**.

   ![Configuration de la stratégie d’en-tête](assets/navigation-routing/header-policy.png)

   Cette opération collecte les 2 niveaux de navigation profonds sous `/content/wknd-spa-react/us/en`.

1. Après avoir enregistré vos modifications, le `Header` renseigné doit s’afficher dans le modèle :

   ![Composant d’en-tête renseigné](assets/navigation-routing/populated-header.png)

## Créer des pages enfants

Créez ensuite des pages supplémentaires dans AEM qui serviront de vues différentes dans le SPA. Nous examinerons également la structure hiérarchique du modèle JSON fourni par AEM.

1. Accédez à la console **Sites** : [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). Sélectionnez **Page d’accueil de React SPA WKND** et cliquez sur **Créer** > **Page** :

   ![Créer une page](assets/navigation-routing/create-new-page.png)

1. Sous **Modèle**, sélectionnez **SPA Page**. Sous **Propriétés**, saisissez **Page 1** pour le **titre** et **page-1** comme nom.

   ![Renseignez les propriétés initiales de la page](assets/navigation-routing/initial-page-properties.png)

   Cliquez sur **Créer** puis, dans la fenêtre contextuelle de la boîte de dialogue, cliquez sur **Ouvrir** pour ouvrir la page dans l’éditeur SPA d’AEM.

1. Ajoutez un nouveau composant **Texte** au **Conteneur de mises en page** principal. Modifiez le composant et saisissez le texte : **Page 1** à l’aide de l’éditeur de texte enrichi et de l’élément **H1** (vous devrez passer en mode plein écran pour modifier les éléments de paragraphe)

   ![Exemple de contenu page 1](assets/navigation-routing/page-1-sample-content.png)

   N’hésitez pas à ajouter du contenu supplémentaire, comme une image.

1. Revenez à la console AEM Sites et répétez les étapes ci-dessus, en créant une seconde page nommée **Page 2** en tant que frère de **Page 1**.
1. Enfin, créez une troisième page, **Page 3** mais en tant que **enfant** de **Page 2**. Une fois la hiérarchie du site terminée, elle doit se présenter comme suit :

   ![Exemple de hiérarchie de site](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. Dans un nouvel onglet, ouvrez l’API de modèle JSON fournie par AEM : [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). Ce contenu JSON est demandé lors du premier chargement de la SPA. La structure extérieure ressemble à ce qui suit :

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

   Sous `:children`, une entrée doit s’afficher pour chacune des pages créées. Le contenu de toutes les pages figure dans cette requête JSON initiale. Une fois que le routage de navigation est implémenté, les vues suivantes du SPA sont chargées rapidement, puisque le contenu est déjà disponible côté client.

   Il n’est pas judicieux de charger **ALL** du contenu d’un SPA dans la requête JSON initiale, car cela ralentirait le chargement initial de la page. Examinons ensuite la manière dont la profondeur d’hiérarchie des pages est collectée.

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

   Notez que le chemin **Page 3** a été supprimé : `/content/wknd-spa-react/us/en/home/page-2/page-3` du modèle JSON initial.

   Plus tard, nous verrons comment le SDK de l’AEM SPA éditeur peut charger dynamiquement du contenu supplémentaire.

## Mise en oeuvre de la navigation

Implémentez ensuite le menu de navigation dans le cadre de la balise `Header`. Nous pourrions ajouter le code directement dans `Header.js`, mais une meilleure pratique avec consiste à éviter les composants volumineux. Au lieu de cela, nous implémenterons un composant `Navigation` SPA qui pourrait être réutilisé ultérieurement.

1. Examinez le fichier JSON exposé par le composant `Header` AEM à l’adresse [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) :

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

   La nature hiérarchique des pages d’AEM est modélisée dans le code JSON qui peut être utilisé pour renseigner un menu de navigation. Rappelez-vous que le composant `Header` hérite de toutes les fonctionnalités du [composant principal de navigation](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html) et que le contenu exposé via le JSON est automatiquement mappé aux props React.

1. Ouvrez une nouvelle fenêtre de terminal et accédez au dossier `ui.frontend` du projet SPA. Démarrez **webpack-dev-server** avec la commande `npm start`.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

1. Ouvrez un nouvel onglet du navigateur et accédez à [http://localhost:3000/](http://localhost:3000/).

   **webpack-dev-server** doit être configuré pour remplacer le modèle JSON à partir d’une instance locale d’AEM (`ui.frontend/.env.development`). Cela nous permettra de coder directement par rapport au contenu créé dans AEM lors de l’exercice précédent. Assurez-vous d’être authentifié dans AEM dans la même session de navigation.

   ![basculement de menu](./assets/navigation-routing/nav-toggle-static.gif)

   La fonction de basculement de menu de `Header` est actuellement mise en oeuvre. Implémentez ensuite le menu de navigation.

1. Revenez à l’IDE de votre choix et ouvrez la balise `Header.js` à l’adresse `ui.frontend/src/components/Header/Header.js`.
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

   Le code ci-dessus renseigne une URL en fonction de l’élément de navigation racine configuré par le composant. `homeLink()` est utilisé pour renseigner le logo dans la  `logo()` méthode et pour déterminer si le bouton Précédent doit être affiché dans  `backButton()`.

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

   Comme mentionné précédemment, au lieu d’implémenter la navigation dans `Header`, nous implémenterons la majorité de la logique dans le composant `Navigation`.  Les props de `Header` incluent la structure JSON nécessaire pour créer le menu. Nous transmettons toutes les props.
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

   Cette méthode utilise un tableau d’éléments de navigation, `children`, et crée une liste non ordonnée. Il effectue ensuite une itération sur le tableau et transmet l’élément à la balise `renderNavItem`, qui sera ensuite implémentée.

1. Implémentez le `renderNavItem` :

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

   Cette méthode effectue le rendu d’un élément de liste, avec des classes CSS basées sur les propriétés `level` et `active`. La méthode appelle ensuite `renderLink` pour créer la balise d’ancrage. Étant donné que le contenu `Navigation` est hiérarchique, une stratégie récursive est utilisée pour appeler la balise `renderGroupNav` pour les enfants de l’élément actif.

1. Implémentez la méthode `renderLink` :

   Ajoutez une méthode d&#39;import pour le composant [Lien](https://reacttraining.com/react-router/web/api/Link), faisant partie du routeur React, en haut du fichier avec les autres importations :

   ```js
   import {Link} from "react-router-dom";
   ```

   Terminez ensuite l’implémentation de la méthode `renderLink` :

   ```js
   renderLink(item){
       return (
           <Link to={item.url} title={item.title} aria-current={item.active && 'page'}
              className={this.baseCss + '__item-link'}>{item.title}</Link>
       );
   }
   ```

   Notez qu’à la place d’une balise d’ancrage normale, `<a>`, le composant [Lien](https://reacttraining.com/react-router/web/api/Link) est utilisé. Cela permet de s’assurer que l’actualisation de la page entière n’est pas déclenchée et exploite à la place le routeur React fourni par le SDK JS de l’éditeur SPA d’AEM.

1. Enregistrez les modifications dans `Navigation.js` et revenez à **webpack-dev-server** : [http://localhost:3000](http://localhost:3000)

   ![Navigation dans l’en-tête terminée](assets/navigation-routing/completed-header.png)

   Ouvrez la navigation en cliquant sur le bouton bascule du menu et les liens de navigation renseignés doivent s’afficher. Vous devriez être en mesure d’accéder à différentes vues de la SPA.

## Inspect du routage SPA

Maintenant que la navigation a été mise en oeuvre, inspectez le routage dans AEM.

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

1. Ouvrez un terminal, accédez à la racine du projet et déployez le projet pour AEM à l’aide de vos compétences Maven :

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Accédez à la page d’accueil SPA dans AEM : [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) et ouvrez les outils de développement de votre navigateur. Les captures d’écran ci-dessous sont capturées à partir du navigateur Google Chrome.

   Actualisez la page. Une requête XHR doit s’afficher sur `/content/wknd-spa-react/us/en.model.json`, qui est la racine SPA. Notez que seules trois pages enfants sont incluses en fonction de la configuration de la profondeur de hiérarchie du modèle racine SPA effectué plus tôt dans le tutoriel. Cela n’inclut pas **Page 3**.

   ![Requête JSON initiale - SPA racine](assets/navigation-routing/initial-json-request.png)

1. Les outils de développement étant ouverts, utilisez la navigation `Header` pour accéder à **Page 3** :

   ![Navigation à la page 3](assets/navigation-routing/page-three-navigation.png)

   Notez qu’une nouvelle requête XHR est envoyée à : `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![Page trois - Requête XHR](assets/navigation-routing/page-3-xhr-request.png)

   AEM Model Manager comprend que le contenu JSON **Page 3** n’est pas disponible et déclenche automatiquement la requête XHR supplémentaire.

1. Continuez à parcourir les SPA à l’aide des différents liens de navigation du composant `Header`. Notez qu’aucune requête XHR supplémentaire n’est effectuée et qu’aucune actualisation de page complète n’a lieu. Cela permet à l’utilisateur final de SPA plus rapidement et de réduire les requêtes inutiles à AEM.

   ![Navigation implémentée](assets/navigation-routing/final-navigation-implemented.gif)

1. Testez les liens profonds en accédant directement à : [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). Observez que le bouton Précédent du navigateur continue de fonctionner.

## Félicitations !  {#congratulations}

Félicitations, vous avez appris comment plusieurs vues dans la SPA peuvent être prises en charge en mappant sur AEM pages avec le SDK de l’éditeur de formulaires. La navigation dynamique a été mise en oeuvre à l’aide de React Router et ajoutée au composant `Header`.

Vous pouvez toujours afficher le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/navigation-routing-solution) ou extraire le code localement en passant à la branche `React/navigation-routing-solution`.
