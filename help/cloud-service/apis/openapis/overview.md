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
source-git-commit: 58ae9e503bd278479d78d4df6ffe39356d5ec59b
workflow-type: ht
source-wordcount: '1100'
ht-degree: 100%

---

# API AEM basées sur OpenAPI

>[!IMPORTANT]
>
>Les API AEM basées sur OpenAPI sont disponibles uniquement dans AEM as a Cloud Service et ne sont pas compatibles avec AEM 6.X.

Découvrez les API AEM basées sur OpenAPI, notamment la prise en charge de l’authentification, les concepts clés et comment accéder aux API Adobe.

La [spécification OpenAPI](https://swagger.io/specification/) (anciennement connue sous le nom de Swagger) est une norme largement utilisée pour définir les API RESTful. AEM as a Cloud Service fournit plusieurs API basées sur la spécification OpenAPI (ou simplement des API AEM basées sur OpenAPI), ce qui facilite la création d’applications personnalisées qui interagissent avec les types de services de création ou de publication d’AEM. Voici quelques exemples :

**Sites**

- [API Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/) : API permettant d’utiliser des fragments de contenu.

**Assets**

- [API de dossiers](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/) : API permettant d’utiliser des dossiers tels que les dossiers de création, de liste et de suppression.

- [API de création Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) : API permettant d’utiliser des ressources et leurs métadonnées.

**Forms**

- [API Forms Communications](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) : API permettant d’utiliser des formulaires et des documents.

Dans les prochaines versions, d’autres API AEM basées sur OpenAPI seront ajoutées pour prendre en charge d’autres cas d’utilisation.

## Prise en charge de l’authentification{#authentication-support}

Les API AEM basées sur OpenAPI prennent en charge l’authentification OAuth 2.0, y compris les types d’octroi suivants :

- **Informations d’identification de serveur à serveur OAuth** : option idéale pour les services principaux nécessitant un accès à l’API sans interaction avec l’utilisateur ou l’utilisatrice. Elle utilise le type d’octroi _client_credentials_, permettant une gestion sécurisée des accès au niveau du serveur. Pour plus d’informations, consultez [Informations d’identification de serveur à serveur OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Informations d’identification de l’application web OAuth** : option adaptée aux applications web avec des composants front-end et _back-end_ qui accèdent aux API AEM pour le compte des utilisateurs et utilisatrices. Elle utilise le type d’octroi _authorization_code_, où le serveur principal gère en toute sécurité les secrets et les jetons. Pour plus d’informations, consultez [Informations d’identification de l’application web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential).

- **Informations d’identification d’application d’une seule page OAuth** : option conçue pour les SPA s’exécutant dans le navigateur, qui doit accéder aux API au nom d’un utilisateur ou d’une utilisatrice sans serveur principal. Elle utilise le type d’octroi _authorization_code_ et s’appuie sur des mécanismes de sécurité côté client utilisant PKCE (Proof Key for Code Exchange) pour sécuriser le flux de code d’autorisation. Pour plus d’informations, consultez [Informations d’identification d’application d’une seule page OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential).

## Quelle méthode d’authentification utiliser{#auth-method-decision}

Lorsque vous décidez de la méthode d’authentification à utiliser, tenez compte des points suivants :

![Quelle méthode d’authentification utiliser ?](./assets/overview/which-authentication-method-to-use.png)

L’authentification de l’utilisateur ou de l’utilisatrice (application web ou application d’une seule page) doit être le choix par défaut chaque fois que le contexte d’utilisation AEM est impliqué. Cela permet de s’assurer que toutes les actions du référentiel sont correctement attribuées à la personne authentifiée et que celle-ci est limitée aux seules autorisations auxquelles elle a droit.
L’utilisation de l’option serveur à serveur (ou d’un compte système technique) pour effectuer des actions au nom d’une personne individuelle contourne le modèle de sécurité et introduit des risques tels que la réaffectation des privilèges et un audit inexact.

## Différence entre les informations d’identification de serveur à serveur, d’application web et d’application d’une seule page OAuth{#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials}

Le tableau suivant résume les différences entre les trois méthodes d’authentification OAuth prises en charge par les API AEM basées sur OpenAPI :

|  | OAuth serveur à serveur | Application Web OAuth | OAuth application d’une seule page (SPA) |
| --- | --- | --- | --- |
| **Objectif de l’authentification** | Conçue pour les interactions entre machines. | Conçue pour les interactions pilotées par l’utilisateur ou l’utilisatrice dans une application web avec un _serveur principal_. | Conçue pour les interactions pilotées par l’utilisateur ou l’utilisatrice dans une _application JavaScript côté client_. |
| **Comportement des jetons** | Émet des jetons d’accès qui représentent l’application cliente elle-même. | Émet des jetons d’accès au nom d’une personne authentifiée _via un serveur principal_. | Émet des jetons d’accès au nom d’une personne authentifiée _via un flux frontal uniquement_. |
| **Cas d’utilisation** | Services principaux nécessitant un accès à l’API sans interaction avec l’utilisateur ou l’utilisatrice. | Applications web avec composants frontaux et principaux accédant aux API pour le compte des utilisateurs et utilisatrices. | Applications frontales pures (JavaScript) accédant aux API pour le compte d’utilisateurs et d’utilisatrices sans serveur principal. |
| **Remarques relatives à la sécurité** | Stockez en toute sécurité les informations d’identification sensibles (`client_id`, `client_secret`) dans les systèmes principaux. | Après l’authentification de la personne, celle-ci se voit accorder son propre _jeton d’accès temporaire via un appel du serveur principal_. Stockez en toute sécurité les informations d’identification sensibles (`client_id`, `client_secret`) dans les systèmes principaux pour échanger le code d’autorisation contre le jeton d’accès. | Après l’authentification de la personne, celle-ci se voit accorder son propre _jeton d’accès temporaire via un appel frontal_. N’utilise pas `client_secret`, car le stockage dans les applications frontales n’est pas sécurisé. Dépend de PKCE pour échanger le code d’autorisation contre le jeton d’accès. |
| **Type d’octroi** | _client_credentials_ | _authorization_code_ | _authorization_code_ avec **PKCE** |
| **Type d’informations d’identification Adobe Developer Console** | OAuth serveur à serveur | Application Web OAuth | Application d’une seule page OAuth |
| **Tutoriel** | [Appeler l’API à l’aide de l’authentification de serveur à serveur](./use-cases/invoke-api-using-oauth-s2s.md) | [Appeler l’API à l’aide de l’authentification d’application web](./use-cases/invoke-api-using-oauth-web-app.md) | [Appeler l’API à l’aide de l’authentification d’application d’une seule page](./use-cases/invoke-api-using-oauth-single-page-app.md) |

## Accès aux API Adobe et concepts associés{#accessing-adobe-apis-and-related-concepts}

Avant d’accéder aux API Adobe, il est essentiel de connaître les éléments clés suivants :

- **[Adobe Developer Console](https://developer.adobe.com/)** : hub de développement permettant d’accéder aux API, aux SDK, aux événements en temps réel et aux fonctions sans serveur d’Adobe, etc. Notez qu’elle est différente de l’_AEM_ Developer Console, qui est utilisée pour le débogage des applications AEM.

- **[Projet Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)** : emplacement central pour la gestion des intégrations d’API, des événements et des fonctions d’exécution. Ici, vous configurez les API, définissez l’authentification et générez les informations d’identification requises.

- **[Profils de produit](https://helpx.adobe.com/fr/enterprise/using/manage-product-profiles.html)** : les profils de produit fournissent un paramètre d’autorisation prédéfini qui vous permet de contrôler l’accès des utilisateurs et utilisatrices ou des applications aux produits Adobe tels qu’AEM, Adobe Target, Adobe Analytics, etc. Chaque produit Adobe est associé à des profils de produit prédéfinis.

- **Services** : les services définissent les autorisations réelles et sont associés au profil de produit. Pour réduire ou augmenter le paramètre prédéfini des autorisations, vous pouvez désélectionner ou sélectionner les services associés au profil de produit. Vous pouvez ainsi contrôler le niveau d’accès au produit et à ses API. Dans AEM as a Cloud Service, les services représentent des groupes d’utilisateurs et d’utilisatrices disposant de listes de contrôle d’accès (ACL) prédéfinies pour les nœuds de référentiel, ce qui permet une gestion des autorisations granulaire.

## Commencer

Découvrez comment configurer votre environnement AEM as a Cloud Service et un projet Adobe Developer Console pour activer l’accès aux API AEM basées sur OpenAPI. Accédez également à l’API AEM à l’aide du navigateur pour vérifier la configuration et examiner la demande et la réponse.

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
                    <p class="is-size-6">Découvrez comment configurer votre environnement AEM as a Cloud Service pour activer l’accès aux API AEM basées sur OpenAPI.</p>
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

Découvrez comment utiliser les API AEM basées sur OpenAPI à l’aide de différentes méthodes d’authentification OAuth :

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
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification de l’application web">Appeler l’API à l’aide de l’authentification d’application web</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application web personnalisée à l’aide de l’authentification de l’application Web OAuth.</p>
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
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="Appeler l’API à l’aide de l’authentification par application d’une seule page" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="Appeler l’API à l’aide de l’authentification par application d’une seule page"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="Appeler l’API à l’aide de l’authentification par application d’une seule page">Appeler l’API à l’aide de l’authentification d’application d’une seule page</a>
                    </p>
                    <p class="is-size-6">Découvrez comment appeler les API AEM basées sur OpenAPI à partir d’une application d’une seule page (SPA) personnalisée à l’aide de l’authentification d’application d’une seule page OAuth.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
