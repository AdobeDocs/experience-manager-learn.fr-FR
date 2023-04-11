---
title: Composant web/JS - AEM exemple sans affichage
description: Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application Web Component/JS explique comment interroger du contenu à l’aide des API GraphQL AEM à l’aide de requêtes persistantes.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 7%

---


# Composant Web

Les exemples d’applications sont un excellent moyen d’explorer les fonctionnalités d’Adobe Experience Manager (AEM) sans interface utilisateur. Cette application de composant Web explique comment interroger le contenu à l’aide des API GraphQL d’AEM à l’aide de requêtes persistantes et effectuer le rendu d’une partie de l’interface utilisateur, à l’aide du code JavaScript pur.

![Composant web avec AEM sans affichage](./assets/web-component/web-component.png)

Afficher la variable [code source sur GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (si vous vous connectez au SDK local AEM 6.5 ou AEM)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## Configuration requise AEM

Le composant Web fonctionne avec les options de déploiement AEM suivantes.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=fr)
+ Configuration locale à l’aide de [le SDK AEM Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr?lang=en#install-local-aem-instances)

Tous les déploiements nécessitent le `tutorial-solution-content.zip` de la [Fichiers de solution](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) à installer et nécessaire [Configurations de déploiement](../deployment/web-component.md) sont effectuées.


>[!IMPORTANT]
>
>Le composant Web est conçu pour se connecter à une __Publication AEM__ , mais il peut toutefois sources du contenu à partir de l’auteur AEM si l’authentification est fournie dans le [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) fichier .

## Utilisation

1. Cloner le `adobe/aem-guides-wknd-graphql` référentiel :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Accédez à `web-component` sous-répertoire .

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. Modifiez la variable `.../src/person.js` afin d’inclure les détails de connexion AEM :

   Dans le `aemHeadlessService` , mettez à jour l’objet `aemHost` pour pointer vers votre service AEM Publish.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   Si vous vous connectez à un service AEM Author, dans la variable `aemCredentials` , fournissez les informations d’identification de l’utilisateur AEM local.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. Ouvrez un terminal et exécutez les commandes à partir de `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. Une nouvelle fenêtre de navigateur ouvre la page de HTML statique qui incorpore le composant Web à l’adresse [http://localhost:8080](http://localhost:8080).
1. Le _Informations sur la personne_ Le composant Web s’affiche sur la page Web.

## Le code

Vous trouverez ci-dessous un résumé de la création du composant Web, de la manière dont il se connecte à AEM sans affichage pour récupérer du contenu à l’aide des requêtes persistantes de GraphQL et de la manière dont ces données sont présentées. Le code complet se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Balise de HTML de composant web

Un composant Web réutilisable (ou élément personnalisé) `<person-info>` est ajouté à la variable `../src/assets/aem-headless.html` Page de HTML. Elle prend en charge `host` et `query-param-value` attributs pour piloter le comportement du composant. Le `host` valeurs d’attribut remplacées `aemHost` de `aemHeadlessService` dans `person.js`, et `query-param-value` est utilisé pour sélectionner la personne à rendre.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Implémentation des composants web

Le `person.js` définit la fonctionnalité des composants web. Vous trouverez ci-dessous les principales caractéristiques de cette fonctionnalité.

#### Implémentation de l’élément PersonInfo

Le `<person-info>` l’objet de classe d’élément personnalisé définit la fonctionnalité à l’aide de la fonction `connectedCallback()` méthodes de cycle de vie, association d’une racine fantôme, récupération de la requête persistante GraphQL et manipulation DOM pour créer la structure DOM fantôme interne de l’élément personnalisé.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### Enregistrez le `<person-info>` element

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### Partage de ressources cross-origin (CORS)

Ce composant Web repose sur une configuration CORS basée sur AEM s’exécutant sur l’environnement AEM cible et suppose que la page hôte s’exécute sur `http://localhost:8080` en mode de développement et ci-dessous, vous trouverez un exemple de configuration OSGi CORS pour le service AEM Author local.

Veuillez consulter [configurations de déploiement](../deployment/web-component.md) pour le service AEM correspondant.

![Configuration CORS](assets/react-app/cross-origin-resource-sharing-configuration.png)
