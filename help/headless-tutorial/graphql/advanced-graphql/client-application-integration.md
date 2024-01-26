---
title: Intégration d’applications clientes - Concepts avancés d’AEM Headless - GraphQL
description: Implémentez des requêtes persistantes et intégrez-les dans l’application WKND.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 290
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 100%

---

# Intégration d’applications clientes

Dans le chapitre précédent, vous avez créé et mis à jour des requêtes persistantes à l’aide de l’explorateur GraphiQL.

Ce chapitre décrit les étapes à suivre pour intégrer les requêtes persistantes à l’application cliente WKND (ou application WKND) à l’aide de requêtes HTTP GET dans les **composants React** existants. Il propose également un exercice facultatif pour mettre en pratique vos connaissances d’AEM Headless et votre expertise en codage afin d’améliorer l’application cliente WKND.

## Prérequis {#prerequisites}

Ce document fait partie d’un tutoriel en plusieurs parties. Assurez-vous que les chapitres précédents ont été terminés avant de poursuivre ce chapitre. L’application cliente WKND se connecte au service de publication AEM. Il est donc important que vous **ayez publié les éléments suivants sur le service de publication AEM** :

* Configurations du projet
* Points d’entrée GraphQL
* Modèles de fragment de contenu
* Fragments de contenu créés
* Requêtes persistantes GraphQL

Les _captures d’écran d’IDE de ce chapitre proviennent de [Visual Studio Code](https://code.visualstudio.com/)_.

### Package de solution des chapitres 1 à 4 (facultatif) {#solution-package}

Un package de solution complétant les étapes de l’interface utilisateur d’AEM des chapitres 1 à 4 est mis à disposition pour être installé. Ce package n’est **pas requis** si vous avez terminé les chapitres précédents.

1. Téléchargez [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. Dans AEM, accédez à **Outils** > **Déploiement** > **Packages** pour accéder au **Gestionnaire de packages**.
1. Téléchargez et installez le package (fichier zip) téléchargé à l’étape précédente.
1. Répliquez le package sur le service de publication AEM.

## Objectifs {#objectives}

Dans ce tutoriel, vous apprendrez à intégrer les demandes de requêtes persistantes dans l’exemple d’application React GraphQL WKND à l’aide du [client AEM Headless pour JavaScript](https://github.com/adobe/aem-headless-client-js).

## Cloner et exécuter l’exemple d’application cliente {#clone-client-app}

Pour accélérer le tutoriel, une application React JS de démarrage est fournie.

1. Clonez le référentiel [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Modifiez le fichier `aem-guides-wknd-graphql/advanced-tutorial/.env.development` et définissez `REACT_APP_HOST_URI` de manière à pointer vers votre service de publication AEM cible.

   Mettez à jour la méthode d’authentification si vous vous connectez à une instance de création.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![Environnement de développement d’application React.](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > Les instructions ci-dessus vous permettent de connecter l’application React au **service de publication AEM**. Pour la connexion au **service de création AEM**, vous avez besoin d’un jeton de développement local pour votre environnement cible AEM as a Cloud Service.
   >
   > Il est également possible de connecter l’application à une [instance de création locale à l’aide du SDK d’AEM as a Cloud Service](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) au moyen de l’authentification de base.


1. Ouvrez un terminal et exécutez les commandes :

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. Une nouvelle fenêtre de navigateur doit s’afficher à l’adresse [http://localhost:3000](http://localhost:3000).


1. Cliquez sur **Camping** > **Yosemite Backpacking** pour consulter les détails de l’Adventure Yosemite Backpacking.

   ![Écran de Yosemite Backpacking.](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. Ouvrez les outils de développement du navigateur et examinez la requête `XHR`.

   ![GraphQL POST.](assets/client-application-integration/graphql-persisted-query.png)

   Vous devriez voir les requêtes `GET` envoyées au point d’entrée GraphQL, ainsi que le nom de la configuration du projet (`wknd-shared`), le nom de la requête persistante (`adventure-by-slug`), le nom de la variable (`slug`), la valeur (`yosemite-backpacking`) et les encodages de caractères spéciaux.

>[!IMPORTANT]
>
>    Si vous vous demandez pourquoi la requête de l’API GraphQL est envoyée à l’adresse `http://localhost:3000` et non au domaine du service de publication AEM, consultez la section [Ce qui se passe](../multi-step/graphql-and-react-app.md#under-the-hood) du tutoriel de base.


## Vérifier le code

Dans l’étape [Tutoriel de base - Créer une application React qui utilise les API GraphQL d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=fr#review-the-aemheadless-object), nous avons examiné et amélioré quelques fichiers clés afin d’acquérir des connaissances pratiques. Avant d’améliorer l’application WKND, nous allons examiner les fichiers essentiels.

* [Vérifier l’objet AEMHeadless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=fr#review-the-aemheadless-object)

* [Implémenter pour exécuter les requêtes persistantes GraphQL d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=fr#implement-to-run-aem-graphql-persisted-queries)

### Vérifier le composant React `Adventures`

La vue principale de l’application React WKND répertorie toutes les Adventures. Vous pouvez les filtrer en fonction du type d’Activity, comme _Camping ou Cycling_. Cette vue est générée par le composant `Adventures`. Voici les principaux détails d’implémentation :

* `src/components/Adventures.js` appelle le hook `useAllAdventures(adventureActivity)`. Ici, l’argument `adventureActivity` est de type Activity.

* Le hook `useAllAdventures(adventureActivity)` est défini dans le fichier `src/api/usePersistedQueries.js`. En fonction de la valeur `adventureActivity`, il détermine la requête persistante à appeler. Si la valeur n’est pas null, il appelle `wknd-shared/adventures-by-activity`. Dans le cas contraire, il récupère toutes les Adventures disponibles `wknd-shared/adventures-all`.

* Le hook utilise la fonction principale `fetchPersistedQuery(..)` qui délègue l’exécution de la requête à `AEMHeadless` via `aemHeadlessClient.js`.

* En outre, le hook renvoie uniquement les données appropriées de la réponse GraphQL d’AEM à `response.data?.adventureList?.items`, ce qui permet aux composants React de vue `Adventures` d’être indépendants des structures JSON parentes.

* Une fois la requête exécutée, la fonction de rendu `AdventureListItem(..)` à partir de `Adventures.js` ajoute un élément HTML pour afficher les informations _Image, Trip Length, Price et Titre_.

### Vérifier le composant React `AdventureDetail`

Le composant React `AdventureDetail` effectue le rendu des détails de l’Adventure. Voici les principaux détails d’implémentation :

* `src/components/AdventureDetail.js` appelle le hook `useAdventureBySlug(slug)`. Ici, l’argument `slug` est un paramètre de requête.

* Comme ci-dessus, le hook `useAdventureBySlug(slug)` est défini dans le fichier `src/api/usePersistedQueries.js`. Il appelle la requête persistante `wknd-shared/adventure-by-slug` en effectuant la délégation à `AEMHeadless` via `aemHeadlessClient.js`.

* Une fois la requête exécutée, la fonction de rendu `AdventureDetailRender(..)` à partir de `AdventureDetail.js` ajoute un élément HTML pour afficher les détails de l’Adventure.


## Améliorer le code

### Utiliser la requête persistante `adventure-details-by-slug`

Dans le chapitre précédent, nous avons créé la requête persistante `adventure-details-by-slug`, qui fournit des informations supplémentaires telles que _l’emplacement, l’équipe d’instruction et la personne administratrice_. Remplaçons `adventure-by-slug` par la requête persistante `adventure-details-by-slug` pour effectuer le rendu de ces informations supplémentaires.

1. Ouvrez `src/api/usePersistedQueries.js`.

1. Localisez la fonction `useAdventureBySlug()` et mettez à jour la requête comme suit :

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### Afficher des informations supplémentaires

1. Pour afficher des informations supplémentaires sur l’Adventure, ouvrez `src/components/AdventureDetail.js`.

1. Localisez la fonction `AdventureDetailRender(..)` et mettez à jour la fonction de retour comme suit :

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. Définissez également les fonctions de rendu correspondantes :

   **LocationInfo**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **Emplacement**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **Administrateur**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### Définir de nouveaux styles

1. Ouvrez `src/components/AdventureDetail.scss` et ajoutez les définitions de classe suivantes :

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>Les fichiers mis à jour sont disponibles sous le projet **AEM Guides WKND - GraphQL**. Voir la section [Tutoriel avancé](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial).


Après avoir apporté les améliorations ci-dessus, l’application WKND ressemble à ce qui suit et les outils de développement du navigateur affichent l’appel de la requête persistante `adventure-details-by-slug`.

![Application WKND améliorée.](assets/client-application-integration/Enhanced-WKND-APP.gif)

## Défi d’amélioration (facultatif)

La vue principale de l’application React WKND vous permet de filtrer ces Adventures en fonction du type d’Activity, tel que _Camping ou Cycling_. Cependant, l’équipe commerciale WKND souhaite disposer d’une fonctionnalité de filtrage supplémentaire dépendant de l’_emplacement_. Les exigences sont les suivantes :

* Dans la vue principale de l’application WKND, dans le coin supérieur gauche ou droit, une icône de filtrage par _emplacement_ doit être ajoutée.
* Un clic sur l’icône de filtrage par _emplacement_ doit afficher la liste des emplacements.
* Un clic sur l’option d’emplacement désirée dans la liste doit afficher uniquement les Adventures correspondantes.
* Si une seule Adventure correspond, la vue Détails de l’Adventure s’affiche.

## Félicitations.

Félicitations. Vous avez maintenant terminé l’intégration et l’implémentation des requêtes persistantes dans l’exemple d’application WKND.
