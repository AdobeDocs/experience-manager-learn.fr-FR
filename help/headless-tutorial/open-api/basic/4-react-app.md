---
title: Créer une application React avec l’OpenAPI AEM | Tutoriel Headless Partie 4
description: Prise en main de Adobe Experience Manager (AEM) et OpenAPI. Créez une application React qui récupère le contenu et les données des API de diffusion de fragments de contenu basées sur OpenAPI d’AEM.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 12%

---


# Créer une application React avec les OpenAPI de diffusion de fragments de contenu AEM

Dans ce chapitre, vous découvrirez comment la diffusion de fragments de contenu AEM avec des API OpenAPI peut améliorer l’expérience des applications externes.

Une application React simple est utilisée pour demander et afficher du contenu **Équipe** et **Personne** exposé par la diffusion de fragments de contenu AEM avec les API OpenAPI. L’utilisation de React n’est pas vraiment importante, et l’application externe consommatrice peut être écrite dans n’importe quel framework pour n’importe quelle plateforme, à condition qu’elle puisse envoyer des requêtes HTTP à AEM as a Cloud Service.

## Prérequis

Nous partons du principe que les étapes décrites dans les parties précédentes de ce tutoriel en plusieurs parties ont été terminées.

Les logiciels suivants doivent être installés :

* [Node.js v22 et ultérieure](https://nodejs.org/fr)
* [Visual Studio Code](https://code.visualstudio.com/)

## Objectifs

Découvrez comment :

* Téléchargez et démarrez l’exemple d’application React.
* Appelez la diffusion de fragments de contenu AEM avec les API OpenAPI pour obtenir la liste des équipes et des membres référencés.
* Appelez la diffusion de fragments de contenu AEM avec les API OpenAPI pour récupérer les détails d’un membre de l’équipe.

## Configurer CORS sur AEM as a Cloud Service

Cet exemple d’application React s’exécute localement (sur `http://localhost:3000`) et se connecte à la diffusion de fragments de contenu AEM du service de publication AEM avec les API OpenAPI. Pour autoriser cette connexion, CORS (partage de ressources entre origines multiples) doit être configuré sur le service de publication (ou d’aperçu) AEM.

Suivez les [instructions de la section Configuration d’une SPA s’exécutant sur `http://localhost:3000` pour autoriser les requêtes CORS au service de publication AEM](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains).

### Proxy CORS local

Pour le développement, vous pouvez également exécuter un [proxy CORS local](https://www.npmjs.com/package/local-cors-proxy) qui installe une connexion CORS à AEM.

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

Mettez à jour la valeur `--proxyUrl` vers votre URL de publication (ou de prévisualisation) AEM.

Une fois le proxy CORS local en cours d’exécution, accédez aux API de diffusion de fragments de contenu d’AEM à l’adresse `http://localhost:8010/proxy` pour éviter les problèmes CORS.

## Clonez l’exemple d’application React

Un exemple d’application React qui fait l’objet d’un bouchon est implémenté avec le code requis pour interagir avec la diffusion de fragments de contenu AEM avec les API OpenAPI et afficher les données de l’équipe et de la personne obtenues de leur part.

L’exemple de code source de l’application React est [disponible sur Github.com](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic).

Pour obtenir l’application React :

1. Clonez l’exemple d’application React OpenAPI WKND à partir de [Github.com](https://github.com/adobe/aem-tutorials) à partir de la balise [`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. Accédez au fichier `headless/open-api/basic` et ouvrez-le dans votre IDE.

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. Mettez à jour `.env` pour vous connecter au service de publication AEM as a Cloud Service, car c’est là que nos fragments de contenu sont publiés. Cela peut pointer vers le service d’aperçu AEM si vous souhaitez tester l’application avec le service d’aperçu AEM (et les fragments de contenu y sont publiés).

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   Lors de l’utilisation du [proxy CORS local](#local-cors-proxy), définissez `REACT_APP_HOST_URI` sur `http://localhost:8010/proxy`.

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. Démarrer l’application React

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. L’application React démarre en mode de développement le [http://localhost:3000/](http://localhost:3000/). Les modifications apportées à l’application React tout au long du tutoriel sont répercutées immédiatement dans le navigateur web.

>[!IMPORTANT]
>
>   Cette application React est partiellement implémentée. Suivez les étapes de ce tutoriel pour terminer la mise en œuvre. Les fichiers JavaScript qui doivent être implémentés comportent les commentaires suivants. Veillez à ajouter ou mettre à jour le code dans ces fichiers avec le code spécifié dans ce tutoriel.
>
>
>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>  &#x200B;>  // Tâche : implémentez ceci en suivant les étapes du tutoriel AEM Headless.
>  &#x200B;>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>

## Anatomie de l’application React

L’exemple d’application React comporte trois parties principales qui doivent être mises à jour.

1. Le fichier `.env` contient l’URL du service de publication AEM (ou de prévisualisation).
1. La `src/components/Teams.js` affiche une liste des équipes et de leurs membres.
1. La `src/components/Person.js` affiche les détails d’un seul membre de l’équipe.

## Implémenter la fonctionnalité Équipes

Développez la fonctionnalité pour afficher les équipes et leurs membres sur la vue principale de l’application React. Cette fonctionnalité requiert :

* Un nouveau [hook personnalisé React useEffect](https://react.dev/reference/react/useEffect#useeffect) qui appelle l’API **List all Content Fragments** via une requête de récupération, puis obtient la valeur `fullName` de chaque `teamMember` à afficher.

Une fois l’opération terminée, la vue principale de l’application est renseignée avec les données de l’équipe provenant d’AEM.

![Vue Équipes.](./assets/4/teams.png)

1. Ouvrez `src/components/Teams.js`.

1. Implémentez le composant **Équipes** pour récupérer la liste des équipes à partir de [Répertorier toutes les API de fragments de contenu](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments) et effectuer le rendu du contenu des équipes. Elle se compose des étapes suivantes :

1. Créez un hook `useEffect` qui appelle l’API AEM **Liste de tous les fragments de contenu** et stocke les données dans l’état du composant React.
1. Pour chaque **Équipe** fragment de contenu renvoyé, appelez l’API **Obtenir un fragment de contenu** pour récupérer les détails complets de l’équipe, y compris ses membres et leurs `fullNames`.
1. Restituer les données des équipes à l’aide de la fonction `Team`

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

## Implémenter la fonctionnalité de personne

Lorsque la fonctionnalité [ Équipes ](#implement-teams-functionality) terminée, implémentez la fonctionnalité permettant de gérer l’affichage sur les détails d’un ou d’une membre de l’équipe, ou d’une personne.

![Vue Personne](./assets/4/person.png)

Pour ce faire :

1. Ouvrez `src/components/Person.js`.
1. Dans le composant React `Person`, analysez le paramètre d’itinéraire `id`. Notez que les Itinéraires de l’application React ont été précédemment configurés pour accepter le paramètre d’URL `id` (voir `/src/App.js`).
1. Récupérez les données de personne d’AEM à l’aide de l’[API Get Content Fragment](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment).

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### Obtenir le code terminé

Le code source complet de ce chapitre est [disponible sur Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end).

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## Essayer l’application

Vérifiez l’application [http://localhost:3000/](http://localhost:3000/) et cliquez sur les liens _Membre de l’équipe_. Vous pouvez également ajouter d’autres équipes et/ou membres à l’équipe Alpha en ajoutant des fragments de contenu dans le service de création AEM et en les publiant.

## Ce qui se passe.

Ouvrez la console **Outils de développement > Réseau** du navigateur et **Filtrer** pour récupérer `/adobe/contentFragments` requêtes lorsque vous interagissez avec l’application React.

## Félicitations !

Félicitations. Vous avez créé une application React pour utiliser et afficher des fragments de contenu à partir de la diffusion de fragments de contenu AEM avec les API OpenAPI.
