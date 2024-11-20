---
title: Présentation des API d’AEM
description: Découvrez les différents types d’API dans Adobe Experience Manager (AEM) et obtenez un aperçu des API basées sur la spécification OpenAPI, communément appelées API d’AEM basées sur OpenAPI.
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
source-git-commit: 316e08e6647d6fd731cd49ae1bc139ce57c3a7f4
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 2%

---

# Présentation des API d’AEM{#aem-apis-overview}

Découvrez les différents types d’API as a Cloud Service Adobe Experience Manager (AEM) et obtenez une vue d’ensemble des API d’AEM basées sur la [ OpenAPI Specification (OAS)](https://swagger.io/specification/), communément appelées API d’ basées sur OpenAPI.

AEM as a Cloud Service fournit un large éventail d’API pour la création, la lecture, la mise à jour et la suppression de contenu, de ressources et de formulaires. Ces API permettent aux développeurs de créer des applications personnalisées qui interagissent avec AEM.

Explorons les différents types d’API d’AEM et comprenons les concepts clés de l’accès aux API d’Adobe.

## Types d’API AEM{#types-of-aem-apis}

AEM propose des API héritées et modernes pour interagir avec ses types de services de création et de publication.

- **API héritées** : introduites dans les versions AEM antérieures, les API héritées sont toujours prises en charge pour la compatibilité ascendante.

- **API modernes** : basées sur la spécification REST OpenAPI, ces API suivent les bonnes pratiques de conception d’API actuelles et sont recommandées pour les nouvelles intégrations.


| Type d’API AEM | Spécifications | Disponibilité | Cas d’utilisation | Exemple |
| --- | --- | --- | --- | --- |
| API traditionnelles (non RESTful) | Servlets Sling | AEM 6.X, AEM as a Cloud Service | Intégrations héritées, rétrocompatibilité | [API Query Builder](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) et autres |
| API RESTful | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | opérations CRUD, applications modernes | [API HTTP Assets](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [API REST de workflow](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [exportateur JSON pour Content Services](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) et autres |
| API GRAPHQL | GraphQL | AEM 6.X, AEM as a Cloud Service | CMS sans tête, SPA, applications mobiles | [API GraphQL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| API AEM OpenAPI | REST, OpenAPI | **AEM as a Cloud Service uniquement** | Développement d’abord API, applications modernes | [API d’auteur Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [API de dossiers](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [API AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Services Forms Acrobat](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) et autres |

>[!IMPORTANT]
>
>Les API d’AEM basées sur OpenAPI ne sont disponibles que dans AEM as a Cloud Service et ne sont pas compatibles avec AEM 6.X.

Pour plus d’informations sur les API d’AEM, voir la section [API Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/).

Regardons de plus près les API d’AEM basées sur OpenAPI et les concepts importants d’accès aux API d’Adobe.

## API AEM OpenAPI{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>Les API d’AEM OpenAPI sont disponibles dans le cadre d’un programme d’accès anticipé. Si vous souhaitez y accéder, nous vous encourageons à envoyer un email [aem-apis@adobe.com](mailto:aem-apis@adobe.com) avec une description de votre cas d’utilisation.

La [spécification OpenAPI](https://swagger.io/specification/) (anciennement connue sous le nom de Swagger) est une norme largement utilisée pour définir les API RESTful. AEM as a Cloud Service fournit plusieurs API basées sur les spécifications OpenAPI (ou simplement des API d’AEM basées sur OpenAPI), ce qui facilite la création d’applications personnalisées qui interagissent avec AEM types de services de création ou de publication. Voici quelques exemples :

**Sites**

- [API Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/) : API pour travailler avec des fragments de contenu.

**Assets**

- [API de dossiers](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/) : API pour travailler avec des dossiers tels que créer, répertorier et supprimer des dossiers.

- [API d’auteur Assets](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) : API pour travailler avec des ressources et ses métadonnées.

**Forms**

- [API de communications Forms](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) : API pour travailler avec des formulaires et des documents.

Dans les prochaines versions, d’autres API d’AEM basées sur OpenAPI seront ajoutées pour prendre en charge des cas d’utilisation supplémentaires.

## Prise en charge de l’authentification{#authentication-support}

Les API d’AEM basées sur OpenAPI prennent en charge les méthodes d’authentification suivantes :

- **OAuth Server-to-Server Credential** : idéal pour les services principaux ayant besoin d’un accès à l’API sans interaction de l’utilisateur. Il utilise le type d’octroi _client_credentials_, ce qui permet la gestion sécurisée des accès au niveau du serveur. Pour plus d’informations, voir [Informations d’identification OAuth Server-to-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential).

- **Informations d’identification de l’application web OAuth** : adaptées aux applications web avec interface et _serveur principal_ ayant accès aux API AEM au nom des utilisateurs. Il utilise le type d’octroi _authorization_code_, où le serveur principal gère en toute sécurité les secrets et les jetons. Pour plus d’informations, voir [Informations d’identification de l’application web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential).

- **Informations d’identification de l’application d’une seule page OAuth** : conçues pour SPA s’exécutant dans le navigateur, qui doit accéder aux API au nom d’un utilisateur sans serveur principal. Il utilise le type d’octroi _authorization_code_ et s’appuie sur des mécanismes de sécurité côté client à l’aide de PKCE (BAT Key for Code Exchange) pour sécuriser le flux de code d’autorisation. Pour plus d’informations, voir [Informations d’identification de l’application monopage OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential).

## Accès aux API d’Adobe et aux concepts associés{#accessing-adobe-apis-and-related-concepts}

Avant d’accéder aux API Adobe, il est essentiel de comprendre ces concepts clés :

- **[Adobe Developer Console](https://developer.adobe.com/)** : centre de développement pour l’accès aux API d’Adobe, aux SDK, aux événements en temps réel, aux fonctions sans serveur, etc. Notez qu’il est différent de la Developer Console _AEM_ utilisée pour le débogage des applications AEM.

- **[Projet Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/projects/)** : emplacement central pour la gestion des intégrations API, des événements et des fonctions d’exécution. Ici, vous configurez les API, définissez l’authentification et générez les informations d’identification requises.

- **[Profils de produit](https://helpx.adobe.com/fr/enterprise/using/manage-product-profiles.html)** : les profils de produit fournissent un paramètre d’autorisation prédéfini qui vous permet de contrôler l’accès des utilisateurs ou des applications aux produits d’Adobe tels qu’AEM, Adobe Target, Adobe Analytics, etc. Chaque produit Adobe est associé à des profils de produit prédéfinis.

- **Services** : les services définissent les autorisations réelles et sont associés au profil de produit. Pour réduire ou augmenter le paramètre prédéfini d’autorisations, vous pouvez désélectionner ou sélectionner les services associés au profil de produit. Cela vous permet donc de contrôler le niveau d’accès au produit et à ses API. Dans AEM as a Cloud Service, les services représentent des groupes d’utilisateurs avec des listes de contrôle d’accès (ACL) prédéfinies pour les noeuds de référentiel, ce qui permet une gestion granulaire des autorisations.

## Étapes suivantes{#next-steps}

Comprendre les différents types d’API AEM, notamment
Les API d’AEM basées sur OpenAPI et les concepts clés d’accès aux API d’Adobe vous permettent désormais de commencer à créer des applications personnalisées qui interagissent avec AEM.

Commençons avec le tutoriel [Comment appeler les API d’AEM OpenAPI](invoke-openapi-based-aem-apis.md).
