---
title: Création d’une application React qui demande AEM à l’aide de l’API GraphQL - Prise en main d’AEM sans affichage - GraphQL
description: Prise en main d’Adobe Experience Manager (AEM) et de GraphQL. Créez l’application React qui récupère le contenu/les données de l’API GraphQL d’AEM. Découvrez également comment AEM SDK JS sans affichage est utilisé.
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '1174'
ht-degree: 3%

---


# Création d’une application React qui utilise AEM API GraphQL

Dans ce chapitre, vous découvrirez comment les API GraphQL AEM peuvent piloter l’expérience dans une application externe.

Une application React simple est utilisée pour effectuer des requêtes et afficher des **Équipe** et **Personne** contenu exposé par AEM API GraphQL. L’utilisation de React n’a guère d’importance et l’application externe consommatrice peut être écrite dans n’importe quel framework pour n’importe quelle plateforme.

## Prérequis

Nous partons du principe que les étapes décrites dans les parties précédentes de ce tutoriel en plusieurs parties ont été terminées, ou [tutorial-solution-content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) est installé sur vos services de création et de publication as a Cloud Service AEM.

_Les captures d’écran IDE de ce chapitre proviennent de [Visual Studio Code](https://code.visualstudio.com/)_

Les logiciels suivants doivent être installés :

- [Node.js v14+](https://nodejs.org/en/)
- [npm 6.4+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Visual Studio Code](https://code.visualstudio.com/)

## Objectifs

Découvrez comment :

- Télécharger et démarrer l’exemple d’application React
- Interrogez AEM points d’entrée GraphQL à l’aide de la fonction [AEM SDK JS sans affichage](https://github.com/adobe/aem-headless-client-js)
- Demander l’AEM d’une liste d’équipes et de leurs membres référencés
- AEM de requête pour les détails d’un membre de l’équipe

## Obtention de l’exemple d’application React

Dans ce chapitre, un exemple d’application React en bloc est mis en oeuvre avec le code requis pour interagir avec l’API GraphQL d’AEM, et afficher les données de l’équipe et de la personne obtenues à partir de celle-ci.

L’exemple de code source de l’application React est disponible sur Github.com à l’adresse <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

Pour obtenir l’application React :

1. Cloner l’exemple d’application WKND GraphQL React à partir de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Accédez à `basic-tutorial` et ouvrez-le dans votre IDE.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![React App dans VSCode](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. Mettre à jour `.env.development` pour vous connecter à AEM service de publication as a Cloud Service.

   - Définir `REACT_APP_HOST_URI`Valeur de pour être l’URL de publication de votre AEM as a Cloud Service (ex. `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) et `REACT_APP_AUTH_METHOD`La valeur de `none`
   >[!NOTE]
   >
   > Assurez-vous que vous avez publié la configuration du projet, les modèles de fragment de contenu, les fragments de contenu créés, les points de terminaison GraphQL et les requêtes persistantes des étapes précédentes.
   >
   > Si vous avez effectué les étapes ci-dessus sur le SDK AEM Author local, vous pouvez pointer vers `http://localhost:4502` et `REACT_APP_AUTH_METHOD`La valeur de `basic`.


1. À partir de la ligne de commande, accédez au `aem-guides-wknd-graphql/basic-tutorial` folder

1. Démarrage de l’application React

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. L’application React démarre en mode de développement sur [http://localhost:3000/](http://localhost:3000/). Les modifications apportées à l’application React tout au long du tutoriel sont répercutées immédiatement.

![Mise en oeuvre partielle de React App](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   Il s’agit d’une application React partiellement mise en oeuvre. Les étapes suivantes la mènent à sa fin. Les fichiers JavaScript qui doivent être mis en oeuvre comportent les commentaires suivants. Veillez à ajouter/mettre à jour le code dans ces fichiers avec le code spécifié dans ce tutoriel.
>
>
> //*********************************
>
>  // TODO : Pour ce faire, suivez les étapes du tutoriel AEM sans affichage
>
>  //*********************************



## Anatomie de l’application React

L’exemple d’application React comporte trois parties principales :

1. Le `src/api` contient les fichiers utilisés pour effectuer des requêtes GraphQL à AEM.
   - `src/api/aemHeadlessClient.js` initialise et exporte le client AEM sans tête utilisé pour communiquer avec AEM
   - `src/api/usePersistedQueries.js` implémente [hooks React personnalisés](https://reactjs.org/docs/hooks-custom.html) renvoyer des données d’AEM GraphQL à la variable `Teams.js` et `Person.js` afficher les composants.

1. Le `src/components/Teams.js` affiche une liste des équipes et de leurs membres, à l’aide d’une requête list .
1. Le `src/components/Person.js` affiche les détails d’une seule personne à l’aide d’une requête à résultat unique paramétrée.

## Vérification de l’objet AEMHeadless

Consultez la section `aemHeadlessClient.js` pour savoir comment créer le fichier `AEMHeadless` pour communiquer avec AEM.

1. Ouvrez `src/api/aemHeadlessClient.js`.

1. Passez en revue les lignes 1 à 40 :

   - L&#39;import `AEMHeadless` de la déclaration [AEM client sans affichage pour JavaScript](https://github.com/adobe/aem-headless-client-js), ligne 11.

   - Configuration de l’autorisation en fonction de variables définies dans `.env.development`, ligne 14-22, et l’expression de la fonction de flèche `setAuthorization`, ligne 31-40.

   - Le `serviceUrl` configuration pour les [proxy de développement](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) configuration, ligne 27.

1. Les lignes 42 à 49 sont les plus importantes, car elles instancient la variable `AEMHeadless` et exportez-le pour l’utiliser dans l’application React.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## Mise en oeuvre pour exécuter AEM requêtes persistantes GraphQL

Ouvrez le `usePersistedQueries.js` pour implémenter le fichier générique `fetchPersistedQuery(..)` pour exécuter AEM requêtes persistantes GraphQL. Le `fetchPersistedQuery(..)` utilise la fonction `aemHeadlessClient` de `runPersistedQuery()` pour exécuter la requête de manière asynchrone, en fonction d’un comportement promesse.

Plus tard, React personnalisé `useEffect` hook appelle cette fonction pour récupérer des données spécifiques d’AEM.

1. Dans `src/api/usePersistedQueries.js` **update** `fetchPersistedQuery(..)`, ligne 35, avec le code ci-dessous.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## Mise en oeuvre de la fonctionnalité Équipes

Ensuite, développez la fonctionnalité pour afficher les équipes et leurs membres sur la vue principale de l’application React. Cette fonctionnalité requiert :

- Une nouvelle [crochet personnalisé React useEffect](https://reactjs.org/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` qui appelle la variable `my-project/all-teams` requête persistante, renvoi d’une liste de fragments de contenu d’équipe dans AEM.
- Un composant React à l’adresse `src/components/Teams.js` qui appelle le nouveau React personnalisé `useEffect` et effectue le rendu des données des équipes.

Une fois l’application terminée, la vue principale de l’application est renseignée avec les données de l’équipe provenant d’AEM.

![Vue Équipes](./assets/graphql-and-external-app/react-app__teams-view.png)

### Étapes

1. Ouvrez `src/api/usePersistedQueries.js`.

1. Localisation de la fonction `useAllTeams()`

1. Pour créer une `useEffect` hook qui appelle la requête persistante `my-project/all-teams` via `fetchPersistedQuery(..)`, ajoutez le code suivant. Le point d’extension renvoie également uniquement les données appropriées de la réponse GraphQL AEM à l’adresse `data?.teamList?.items`, ce qui permet aux composants de la vue React d’être indépendants des structures JSON parentes.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. Ouvrez `src/components/Teams.js`

1. Dans le `Teams` Composant React, récupérez la liste des équipes d’AEM à l’aide de la fonction `useAllTeams()` hameçon.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. Effectuez la validation des données basées sur l’affichage, en affichant un message d’erreur ou un indicateur de chargement en fonction des données renvoyées.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. Enfin, effectuez le rendu des données des équipes. Chaque équipe renvoyée par la requête GraphQL est générée à l’aide du `Team` Sous-composant React.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## Mise en oeuvre de la fonctionnalité Personne

Avec le [Fonctionnalités des équipes](#implement-teams-functionality) terminons, mettons en oeuvre la fonctionnalité permettant de gérer l’affichage sur les détails d’un membre de l’équipe ou d’une personne.

Cette fonctionnalité requiert :

- Une nouvelle [crochet personnalisé React useEffect](https://reactjs.org/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` qui appelle le paramètre paramétré `my-project/person-by-name` requête persistante et renvoie un enregistrement de personne unique.

- Un composant React à l’adresse `src/components/Person.js` qui utilise le nom complet d’une personne comme paramètre de requête, appelle le nouveau React personnalisé `useEffect` et effectue le rendu des données de personne.

Une fois l’opération terminée, la sélection du nom d’une personne dans la vue Équipes affiche la personne.

![Personne](./assets/graphql-and-external-app/react-app__person-view.png)

1. Ouvrez `src/api/usePersistedQueries.js`.

1. Localisation de la fonction `usePersonByName(fullName)`

1. Pour créer une `useEffect` hook qui appelle la requête persistante `my-project/all-teams` via `fetchPersistedQuery(..)`, ajoutez le code suivant. Le point d’extension renvoie également uniquement les données appropriées de la réponse GraphQL AEM à l’adresse `data?.teamList?.items`, ce qui permet aux composants de la vue React d’être indépendants des structures JSON parentes.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. Ouvrez `src/components/Person.js`
1. Dans le `Person` Composant React, analysez la variable `fullName` paramètre d’itinéraire, et récupérez les données de personne à partir d’AEM à l’aide de la fonction `usePersonByName(fullName)` hameçon.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. Validation des données en mode d’affichage, affichage d’un message d’erreur ou indicateur de chargement en fonction des données renvoyées.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. Enfin, effectuez le rendu des données de personne.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## Essai de l’application

Vérification de l’application [http://localhost:3000/](http://localhost:3000/) et cliquez sur _Membres_ liens. Vous pouvez également ajouter d’autres équipes et/ou membres à l’équipe Alpha en ajoutant des fragments de contenu dans AEM.

## Sous la porte

Ouvrez le **Outils de développement** > **Réseau** et _Filtrer_ pour `all-teams` , vous remarquerez la requête de l’API GraphQL. `/graphql/execute.json/my-project/all-teams` est contre `http://localhost:3000` et **NOT** par rapport à la valeur de `REACT_APP_HOST_URI`(par exemple, <https://publish-p123-e456.adobeaemcloud.com>), car [configuration du proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) using `http-proxy-middleware` module .


![Requête de l’API GraphQL via Proxy](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


Examinez la `../setupProxy.js` et dans `../proxy/setupProxy.auth.**.js` les fichiers notez comment `/content` et `/graphql` les chemins d’accès sont par proxy et indiquent qu’il ne s’agit pas d’une ressource statique.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

Cependant, il ne s’agit pas d’une option appropriée pour le déploiement en production et vous trouverez plus de détails à l’adresse _Déploiement en production_ .

## Félicitations !{#congratulations}

Félicitations ! Vous avez réussi à créer l’application React afin d’utiliser et d’afficher les données des API GraphQL d’AEM dans le cadre d’un tutoriel de base !
