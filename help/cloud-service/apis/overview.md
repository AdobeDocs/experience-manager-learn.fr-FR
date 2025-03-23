---
title: Présentation des API AEM
description: Découvrez les différents types d’API dans Adobe Experience Manager (AEM) et déterminez quelle API choisir pour votre intégration.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17425
thumbnail: KT-17425.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 5%

---

# Présentation des API AEM{#aem-apis-overview}

Découvrez les différents types d’API dans Adobe Experience Manager (AEM) et déterminez quelle API choisir pour votre intégration.

Pour créer, lire, mettre à jour et supprimer du contenu, des ressources et des formulaires dans AEM, les développeurs peuvent utiliser un large éventail d’API. Ces API permettent aux développeurs de créer des applications personnalisées qui interagissent avec AEM.

Explorons les différents types d’API dans AEM et comprenons quelle API choisir pour votre intégration.

## Types d’API AEM{#types-of-aem-apis}

AEM propose les API suivantes pour interagir avec ses types de services de création et de publication.

| Type d’API AEM | Description | Disponibilité | Cas d’utilisation | Exemples d’API |
| --- | --- | --- | --- | --- |
| API AEM basées sur OpenAPI | API normalisées et lisibles par machine pour Assets, Sites et Forms. | **AEM as a Cloud Service uniquement** | Développement API-First, applications modernes | [API de création Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API Folders](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat Services](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) etc |
| API RESTful | Points d’entrée REST traditionnels pour interagir avec les ressources AEM. | AEM 6.X, AEM as a Cloud Service | Opérations CRUD, applications modernes | [API HTTP Assets](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST Workflow](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [Exportateur JSON pour Content Services](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) etc |
| API GRAPHQL | Optimisé pour récupérer efficacement du contenu structuré avec des requêtes flexibles. | AEM 6.X, AEM as a Cloud Service | CMS découplé, SPA, applications mobiles | [API GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| API traditionnelles (autres que RESTful) | API plus anciennes comme JCR, modèles Sling, Query Builder, etc. | AEM 6.X, AEM as a Cloud Service | Intégrations héritées, rétrocompatibilité | [API Query Builder](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) et autres |

Pour plus d’informations, consultez la page [API Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

## L’API à choisir{#which-api-to-choose}

Lors de la sélection d’une API pour votre intégration, tenez compte des facteurs suivants :

- **Cas d’utilisation** : déterminez si l’API AEM prend en charge votre cas d’utilisation. Dans la mesure du possible, _utilisez des API AEM basées sur OpenAPI_, car elles offrent une approche normalisée et moderne de l’interaction avec AEM. Si les API OpenAPI ne sont pas disponibles, pensez à utiliser des API RESTful ou des API GraphQL et, en dernier recours, des API traditionnelles.

- **Compatibilité** : assurez-vous que l’API sélectionnée est compatible avec votre version d’AEM. Par exemple, les API AEM _basées sur OpenAPI) sont exclusives à AEM as a Cloud Service_ et ne sont pas disponibles dans AEM 6.X.

- **Type de service AEM : création ou publication** : le choix de l’API dépend également de son exécution sur le service de création ou de publication, car leurs modèles d’accès sont différents. Le service de création AEM est utilisé pour la création de contenu et requiert toujours une authentification. Le service de publication AEM est utilisé pour la diffusion de contenu et peut ne pas nécessiter d’authentification, selon le cas d’utilisation.

- **Authentification** : vérifiez que l’API prend en charge la méthode d’authentification que vous prévoyez d’utiliser. Par exemple :
   - **API AEM basées sur OpenAPI** : prennent en charge l’authentification OAuth 2.0, y compris les types d’octroi Informations d’identification client (serveur à serveur), Code d’autorisation (application web) et Clé de BAT pour l’échange de code (application d’une seule page). Les autres API d’AEM ne prennent pas en charge l’authentification OAuth 2.0.
   - **API RESTful** : prend en charge l’authentification JSON Web Token (JWT), également appelée authentification par jeton.

## Différence entre le jeton web JSON (JWT) et OAuth 2.0{#difference-between-jwt-and-oauth}

Comparons le jeton web JSON (JWT) et OAuth 2.0, deux mécanismes d’authentification courants utilisés dans les API AEM :

| Fonctionnalité | Jeton Web JSON (JWT) | OAuth 2.0 |
| --- | --- | --- |
| Utilisé dans | API RESTful | API AEM basées sur OpenAPI (non prises en charge dans RESTful ou d’autres API) |
| Objectif | Authentification du service | Authentification de l’utilisateur ou du service |
| Interaction de l’utilisateur | Aucune interaction utilisateur requise | Interaction utilisateur requise pour les types d’octroi Code d’autorisation et Application monopage |
| Mieux Adapté À | Appels d’API serveur à serveur | Accès sécurisé et autorisé pour les applications et les utilisateurs |
| Informations requises | Clé privée pour la signature du JWT | ID client et secret client pour OAuth 2.0 |
| Expiration du jeton | De courte durée, nécessite souvent une actualisation | Le jeton d’accès est de courte durée. Le jeton d’actualisation est de longue durée et utilisé pour obtenir un nouveau jeton d’accès |
| Gestion des informations d’identification | [AEM Developer Console.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console) | [Adobe Developer Console](https://developer.adobe.com/developer-console/) |

## API AEM basées sur OpenAPI

Pour en savoir plus sur les API AEM basées sur OpenAPI et les concepts importants de l’accès aux API Adobe, consultez le guide [API AEM basées sur OpenAPI](./openapis/overview.md).

### Cas d’utilisation

<!-- CARDS
{target = _self}

* ./openapis/use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./openapis/assets/s2s/OAuth-S2S.png}
* ./openapis/use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./openapis/assets/web-app/OAuth-WebApp.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" title="Appeler l’API à l’aide de l’authentification de serveur à serveur" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/s2s/OAuth-S2S.png" alt="Appeler l’API à l’aide de l’authentification de serveur à serveur"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification de serveur à serveur">Appeler l’API à l’aide de l’authentification de serveur à serveur</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application NodeJS personnalisée à l’aide de l’authentification de serveur à serveur OAuth.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" title="Appeler l’API à l’aide de l’authentification de l’application web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/web-app/OAuth-WebApp.png" alt="Appeler l’API à l’aide de l’authentification de l’application web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification de l’application web">Appeler l’API à l’aide de l’authentification des applications web</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application web personnalisée à l’aide de l’authentification par application web OAuth.</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## API GraphQL - Exemples

Pour en savoir plus sur les API GraphQL et leur utilisation, consultez la [Prise en main d’AEM découplé - GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview)

### Cas d’utilisation

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app
  {title = Single Page Application (SPA)}
  {description = Learn how to build a Single Page Application (SPA) that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/react-app-card.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps
  {title = Mobile App}
  {description = Learn how to build a mobile app that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/ios-app-card.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component
  {title = Web Component}
  {description = Learn how to build a web component that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/web-component-card.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single Page Application (SPA)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" title="Application monopage (SPA)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/react-app-card.png" alt="Application monopage (SPA)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" title="Application monopage (SPA)">Application monopage (SPA)</a>
                    </p>
                    <p class="is-size-6">Découvrez comment créer une application monopage (SPA) qui récupère du contenu d’AEM à l’aide d’API GraphQL.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" title="Application mobile" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/ios-app-card.png" alt="Application mobile"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" title="Application mobile"> Application mobile </a>
                    </p>
                    <p class="is-size-6">Découvrez comment créer une application mobile qui récupère du contenu d’AEM à l’aide d’API GraphQL.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" title="Composant web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-component-card.png" alt="Composant web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" title="Composant web">Composant web</a>
                    </p>
                    <p class="is-size-6">Découvrez comment créer un composant web qui récupère du contenu d’AEM à l’aide d’API GraphQL.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## API RESTful - Exemples

En savoir plus sur les API RESTful, telles que l’[API HTTP Assets](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) et l’[exportateur JSON](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter).

### Cas d’utilisation

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to build a native mobile app that fetches content from AEM using Content Services RESTful APIs.}
  {image = ./assets/RESTful-Content-Service.png}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview
  {title = Token-based Authentication for RESTful APIs}
  {description = Learn how to invoke RESTful APIs using JSON Web Token (JWT) authentication.}
  {image = ./assets/RESTful-TokenAuth.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" title="Appeler l’API à l’aide de l’authentification de serveur à serveur" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-Content-Service.png" alt="Appeler l’API à l’aide de l’authentification de serveur à serveur"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification de serveur à serveur">Appeler l’API à l’aide de l’authentification de serveur à serveur</a>
                    </p>
                    <p class="is-size-6">Découvrez comment créer une application mobile native qui récupère du contenu d’AEM à l’aide des API RESTful Content Services.</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Token-based Authentication for RESTful APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" title="Authentification basée sur les jetons pour les API RESTful" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-TokenAuth.png" alt="Authentification basée sur les jetons pour les API RESTful"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" title="Authentification basée sur les jetons pour les API RESTful">Authentification basée sur les jetons pour les API RESTful</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API RESTful à l’aide de l’authentification JSON Web Token (JWT).</p>
                </div>
                <a href="https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


