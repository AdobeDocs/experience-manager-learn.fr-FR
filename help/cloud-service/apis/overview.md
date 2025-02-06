---
title: Présentation des API AEM
description: Découvrez les différents types d’API dans Adobe Experience Manager (AEM) et obtenez un aperçu des API basées sur la spécification OpenAPI, communément appelées API AEM basées sur OpenAPI.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 2b5f7a033921270113eb7f41df33444c4f3d7723
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 2%

---

# Présentation des API AEM{#aem-apis-overview}

Découvrez les différents types d’API dans Adobe Experience Manager (AEM) as a Cloud Service et obtenez un aperçu des API AEM basées sur la [spécification OpenAPI (OAS)](https://swagger.io/specification/), communément appelées API AEM basées sur OpenAPI.

AEM as a Cloud Service fournit un large éventail d’API pour la création, la lecture, la mise à jour et la suppression de contenu, de ressources et de formulaires. Ces API permettent aux développeurs de créer des applications personnalisées qui interagissent avec AEM.

Explorons les différents types d’API dans AEM et comprenons les concepts clés de l’accès aux API Adobe.

## Types d’API AEM{#types-of-aem-apis}

AEM propose des API héritées et modernes pour interagir avec ses types de services de création et de publication.

- **API héritées** : introduites dans les versions antérieures d’AEM, les API héritées sont toujours prises en charge à des fins de rétrocompatibilité.

- **API modernes** : en fonction de la spécification REST, OpenAPI, ces API suivent les bonnes pratiques de conception d’API actuelles et sont recommandées pour les nouvelles intégrations.


| Type d’API AEM | Spécifications | Disponibilité | Cas d’utilisation | Exemple |
| --- | --- | --- | --- | --- |
| API traditionnelles (autres que RESTful) | Servlets Sling | AEM 6.X, AEM as a Cloud Service | Intégrations héritées, rétrocompatibilité | [API Query Builder](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) et autres |
| API RESTful | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | Opérations CRUD, applications modernes | [API HTTP Assets](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST Workflow](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [Exportateur JSON pour Content Services](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) etc |
| API GRAPHQL | GraphQL | AEM 6.X, AEM as a Cloud Service | CMS découplé, SPA, applications mobiles | [API GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| API AEM OpenAPI | REST, OpenAPI | **AEM as a Cloud Service uniquement** | Développement API-First, applications modernes | [API de création Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API Folders](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat Services](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) etc |

>[!IMPORTANT]
>
>Les API AEM basées sur OpenAPI sont disponibles uniquement dans AEM as a Cloud Service et ne sont pas compatibles avec AEM 6.X.

Pour plus d’informations sur les API AEM, consultez les [API Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Examinons de plus près les API AEM basées sur OpenAPI et les concepts importants de l’accès aux API Adobe.

## API AEM OpenAPI{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>Les API AEM basées sur OpenAPI sont disponibles dans le cadre d’un programme d’accès anticipé. Si vous souhaitez y accéder, nous vous encourageons à envoyer un e-mail à l’adresse [aem-apis@adobe.com](mailto:aem-apis@adobe.com) avec une description de votre cas d’utilisation.

La [spécification OpenAPI](https://swagger.io/specification/) (anciennement connue sous le nom de Swagger) est une norme largement utilisée pour définir les API RESTful. AEM as a Cloud Service fournit plusieurs API basées sur la spécification OpenAPI (ou simplement des API AEM basées sur OpenAPI), ce qui facilite la création d’applications personnalisées qui interagissent avec les types de services de création ou de publication AEM. Voici quelques exemples :

**Sites**

- [API Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/) : API permettant d’utiliser des fragments de contenu.

**Assets**

- [API Folders](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/) : API permettant d’utiliser des dossiers tels que la création, la liste et la suppression de dossiers.

- [API de création Assets ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) : API permettant d’utiliser des ressources et leurs métadonnées.

**Forms**

- [API Forms Communications](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) : API permettant d’utiliser des formulaires et des documents.

Dans les prochaines versions, d’autres API AEM basées sur OpenAPI seront ajoutées pour prendre en charge d’autres cas d’utilisation.

### Prise en charge de l’authentification{#authentication-support}

Les API AEM basées sur OpenAPI prennent en charge les méthodes d’authentification suivantes :

- **Informations d’identification de serveur à serveur OAuth** : idéal pour les services principaux nécessitant un accès à l’API sans interaction de l’utilisateur. Il utilise le type d’octroi _client_credentials_, permettant une gestion sécurisée des accès au niveau du serveur. Pour plus d’informations, voir Informations d’identification de serveur à serveur [OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Informations d’identification de l’application web OAuth** : convient aux applications web avec des composants front-end et _back-end_ qui accèdent aux API AEM pour le compte des utilisateurs. Il utilise le type d’octroi _authorization_code_, où le serveur principal gère en toute sécurité les secrets et les jetons. Pour plus d’informations, voir Informations d’identification de l’application web [OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Informations d’identification d’application d’une seule page OAuth** : conçu pour SPA s’exécutant dans le navigateur, qui doit accéder aux API au nom d’un utilisateur sans serveur principal. Il utilise le type d’octroi _authorization_code_ et s’appuie sur des mécanismes de sécurité côté client utilisant PKCE (Proof Key for Code Exchange) pour sécuriser le flux de code d’autorisation. Pour plus d’informations, voir Informations d’identification d’application d’une seule page [OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

### Différence entre les informations d’identification OAuth de serveur à serveur et OAuth Web App/Single Page App{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | OAuth serveur à serveur | Authentification de l’utilisateur OAuth (web-app) |
| --- | --- | --- |
| Objectif de l’authentification | Conçu pour les interactions entre machines. | Conçu pour les interactions orientées utilisateur. |
| Comportement des jetons | Émet des jetons d’accès qui représentent l’application cliente elle-même. | Émet des jetons d’accès au nom d’un utilisateur authentifié. |
| Cas d’utilisation | Services principaux nécessitant un accès à l’API sans interaction de l’utilisateur. | Applications web avec composants frontaux et principaux accédant aux API pour le compte des utilisateurs. |
| Considérations relatives à la sécurité | Stockez en toute sécurité les informations d’identification sensibles (`client_id`, `client_secret`) dans les systèmes principaux. | Les utilisateurs s’authentifient et se voient accorder leur propre jeton d’accès temporaire. Stockez en toute sécurité les informations d’identification sensibles (`client_id`, `client_secret`) dans les systèmes principaux. |
| Type d’octroi | _client_credentials_ | _code_autorisation_ |

### Accès aux API Adobe et aux concepts associés{#accessing-adobe-apis-and-related-concepts}

Avant d’accéder aux API Adobe, il est essentiel de connaître les concepts clés suivants :

- **[Adobe Developer Console](https://developer.adobe.com/)** : hub de développement permettant d’accéder aux API, aux SDK, aux événements en temps réel et aux fonctions sans serveur d’Adobe, etc. Notez qu’il est différent du Developer Console _AEM_, qui est utilisé pour le débogage des applications AEM.

- **[Projet Adobe Developer Console ](https://developer.adobe.com/developer-console/docs/guides/projects/)** : emplacement central pour la gestion des intégrations d’API, des événements et des fonctions d’exécution. Ici, vous configurez les API, définissez l’authentification et générez les informations d’identification requises.

- **[Profils de produit](https://helpx.adobe.com/fr/enterprise/using/manage-product-profiles.html)** : les profils de produit fournissent un paramètre d’autorisation prédéfini qui vous permet de contrôler l’accès des utilisateurs ou des applications aux produits d’Adobe tels qu’AEM, Adobe Target, Adobe Analytics, etc. Chaque produit d’Adobe est associé à des profils de produit prédéfinis.

- **Services** : les services définissent les autorisations réelles et sont associés au profil de produit. Pour réduire ou augmenter le paramètre prédéfini des autorisations, vous pouvez désélectionner ou sélectionner les services associés au profil de produit. Vous pouvez ainsi contrôler le niveau d’accès au produit et à ses API. Dans AEM as a Cloud Service, les services représentent des groupes d’utilisateurs disposant de listes de contrôle d’accès (ACL) prédéfinies pour les nœuds de référentiel, ce qui permet une gestion des autorisations granulaire.

## Étapes suivantes{#next-steps}

Connaître les différents types d’API AEM, notamment :
Grâce aux API AEM basées sur OpenAPI et aux concepts clés de l’accès aux API Adobe, vous êtes maintenant prêt à créer des applications personnalisées qui interagissent avec AEM.

Commençons par :

- [Appeler les API AEM basées sur OpenAPI pour l’authentification de serveur à serveur](invoke-openapi-based-aem-apis.md) tutoriel qui explique comment accéder aux API AEM basées sur OpenAPI _à l’aide des informations d’identification de serveur à serveur OAuth_.
- [Appeler des API AEM basées sur OpenAPI avec authentification utilisateur à partir d’une application web](invoke-openapi-based-aem-apis-from-web-app.md) tutoriel qui explique comment accéder aux API AEM basées sur OpenAPI à partir d’une _application web à l’aide des informations d’identification d’application web OAuth_.
