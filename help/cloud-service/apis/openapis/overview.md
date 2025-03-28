---
title: API AEM basées sur OpenAPI
description: Découvrez les API AEM basées sur OpenAPI, notamment la prise en charge de l’authentification, les concepts clés et comment accéder aux API Adobe.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 52aad0b0e568ff7e4acd23742fc70f10b1dd14ee
workflow-type: tm+mt
source-wordcount: '885'
ht-degree: 2%

---

# API AEM basées sur OpenAPI

>[!IMPORTANT]
>
>Les API AEM basées sur OpenAPI sont disponibles uniquement dans AEM as a Cloud Service et ne sont pas compatibles avec AEM 6.X.

Découvrez les API AEM basées sur OpenAPI, notamment la prise en charge de l’authentification, les concepts clés et comment accéder aux API Adobe.

La [spécification OpenAPI](https://swagger.io/specification/) (anciennement connue sous le nom de Swagger) est une norme largement utilisée pour définir les API RESTful. AEM as a Cloud Service fournit plusieurs API basées sur la spécification OpenAPI (ou simplement des API AEM basées sur OpenAPI), ce qui facilite la création d’applications personnalisées qui interagissent avec les types de services de création ou de publication d’AEM. Voici quelques exemples :

**Sites**

- [API Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/) : API permettant d’utiliser des fragments de contenu.

**Assets**

- [API Folders](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/) : API permettant d’utiliser des dossiers tels que la création, la liste et la suppression de dossiers.

- [API de création Assets ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) : API permettant d’utiliser des ressources et leurs métadonnées.

**Forms**

- [API Forms Communications](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) : API permettant d’utiliser des formulaires et des documents.

Dans les prochaines versions, d’autres API AEM basées sur OpenAPI seront ajoutées pour prendre en charge d’autres cas d’utilisation.

>[!AVAILABILITY]
>
>Les API AEM basées sur OpenAPI sont disponibles dans le cadre d’un programme d’accès anticipé. Si vous souhaitez y accéder, nous vous encourageons à envoyer un e-mail à l’adresse [aem-apis@adobe.com](mailto:aem-apis@adobe.com) avec une description de votre cas d’utilisation.

## Prise en charge de l’authentification{#authentication-support}

Les API d’AEM basées sur OpenAPI prennent en charge l’authentification OAuth 2.0, y compris les types d’octroi suivants :

- **Informations d’identification de serveur à serveur OAuth** : idéal pour les services principaux nécessitant un accès à l’API sans interaction de l’utilisateur. Il utilise le type d’octroi _client_credentials_, permettant une gestion sécurisée des accès au niveau du serveur. Pour plus d’informations, voir Informations d’identification de serveur à serveur [OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Informations d’identification de l’application web OAuth** : adapté aux applications web avec des composants frontaux et _principaux_ qui accèdent aux API d’AEM pour le compte des utilisateurs et utilisatrices. Il utilise le type d’octroi _authorization_code_, où le serveur principal gère en toute sécurité les secrets et les jetons. Pour plus d’informations, voir Informations d’identification de l’application web [OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Informations d’identification d’application d’une seule page OAuth** : conçu pour les SPA s’exécutant dans le navigateur, qui doit accéder aux API au nom d’un utilisateur sans serveur principal. Il utilise le type d’octroi _authorization_code_ et s’appuie sur des mécanismes de sécurité côté client utilisant PKCE (Proof Key for Code Exchange) pour sécuriser le flux de code d’autorisation. Pour plus d’informations, voir Informations d’identification d’application d’une seule page [OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Différence entre les informations d’identification OAuth de serveur à serveur et OAuth Web App/Single Page App{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | OAuth serveur à serveur | Authentification de l’utilisateur OAuth (web-app) |
| --- | --- | --- |
| Objectif de l’authentification | Conçu pour les interactions entre machines. | Conçu pour les interactions orientées utilisateur. |
| Comportement des jetons | Émet des jetons d’accès qui représentent l’application cliente elle-même. | Émet des jetons d’accès au nom d’un utilisateur authentifié. |
| Cas d’utilisation | Services principaux nécessitant un accès à l’API sans interaction de l’utilisateur. | Applications web avec composants frontaux et principaux accédant aux API pour le compte des utilisateurs. |
| Considérations relatives à la sécurité | Stockez en toute sécurité les informations d’identification sensibles (`client_id`, `client_secret`) dans les systèmes principaux. | Les utilisateurs s’authentifient et se voient accorder leur propre jeton d’accès temporaire. Stockez en toute sécurité les informations d’identification sensibles (`client_id`, `client_secret`) dans les systèmes principaux. |
| Type d’octroi | _client_credentials_ | _code_autorisation_ |

## Accès aux API Adobe et aux concepts associés{#accessing-adobe-apis-and-related-concepts}

Avant d’accéder aux API d’Adobe, il est essentiel de connaître les éléments clés suivants :

- **[Adobe Developer Console](https://developer.adobe.com/)** : hub de développement permettant d’accéder aux API, aux SDK, aux événements en temps réel et aux fonctions sans serveur d’Adobe, etc. Notez qu’il est différent du Developer Console _AEM_, qui est utilisé pour le débogage des applications AEM.

- **[Projet Adobe Developer Console ](https://developer.adobe.com/developer-console/docs/guides/projects/)** : emplacement central pour la gestion des intégrations d’API, des événements et des fonctions d’exécution. Ici, vous configurez les API, définissez l’authentification et générez les informations d’identification requises.

- **[Profils de produit](https://helpx.adobe.com/fr/enterprise/using/manage-product-profiles.html)** : les profils de produit fournissent un paramètre d’autorisation prédéfini qui vous permet de contrôler l’accès des utilisateurs ou des applications aux produits Adobe tels qu’AEM, Adobe Target, Adobe Analytics, etc. Chaque produit Adobe est associé à des profils de produit prédéfinis.

- **Services** : les services définissent les autorisations réelles et sont associés au profil de produit. Pour réduire ou augmenter le paramètre prédéfini des autorisations, vous pouvez désélectionner ou sélectionner les services associés au profil de produit. Vous pouvez ainsi contrôler le niveau d’accès au produit et à ses API. Dans AEM as a Cloud Service, les services représentent des groupes d’utilisateurs disposant de listes de contrôle d’accès (ACL) prédéfinies pour les nœuds de référentiel, ce qui permet une gestion des autorisations granulaire.

## Commencer

Découvrez comment configurer votre environnement AEM as a Cloud Service et un projet Adobe Developer Console pour activer l’accès aux API AEM basées sur OpenAPI. Accédez également à l’API AEM à l’aide du navigateur pour vérifier la configuration et examiner la requête et la réponse.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Configurer les API AEM basées sur OpenAPI" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="Configurer les API AEM basées sur OpenAPI"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Configurer les API AEM basées sur OpenAPI">Configurer les API AEM basées sur OpenAPI</a>
                    </p>
                    <p class="is-size-6">Découvrez comment configurer votre environnement AEM as a Cloud Service pour activer l’accès aux API AEM basées sur OpenAPI.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## Tutoriels sur les API

Découvrez comment utiliser les API AEM basées sur OpenAPI à l’aide de différentes méthodes d’authentification OAuth :

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth Single Page App authentication.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="Appeler l’API à l’aide de l’authentification de serveur à serveur" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="Appeler l’API à l’aide de l’authentification de serveur à serveur"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification de serveur à serveur">Appeler l’API à l’aide de l’authentification de serveur à serveur</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application NodeJS personnalisée à l’aide de l’authentification de serveur à serveur OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Appeler l’API à l’aide de l’authentification de l’application web" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Appeler l’API à l’aide de l’authentification de l’application web"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification de l’application web">Appeler l’API à l’aide de l’authentification des applications web</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application web personnalisée à l’aide de l’authentification par application web OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="Appeler l’API à l’aide de l’authentification par application monopage" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="Appeler l’API à l’aide de l’authentification par application monopage"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification par application monopage">Appeler l’API à l’aide de l’authentification par application monopage</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application d’une seule page (SPA) personnalisée à l’aide de l’authentification par application d’une seule page OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
