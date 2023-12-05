---
title: Déploiements serveur à serveur d’AEM Headless
description: Découvrez les points à prendre en compte pour les déploiements serveur à serveur d’AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 90
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 100%

---

# Déploiements serveur à serveur d’AEM Headless

Les déploiements serveur à serveur d’AEM Headless impliquent des applications ou des processus côté serveur qui consomment et interagissent avec le contenu d’AEM de manière découplée.

Les déploiements serveur à serveur nécessitent une configuration minimale, car les connexions HTTP aux API AEM Headless ne sont pas initiées dans le contexte d’un navigateur.

## Configurations de déploiement

La configuration de déploiement suivante doit être en place pour les déploiements d’applications serveur à serveur.

| L’application serveur à serveur se connecte aux éléments suivants : | Création AEM | Publication AEM | Prévisualisation AEM |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher.](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Partage de ressources entre origines multiples (CORS) | ✘ | ✘ | ✘ |
| [Hôtes AEM.](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exigences d’autorisation

Les requêtes autorisées pour les API GraphQL AEM se produisent généralement dans le contexte des applications serveur à serveur, puisque d’autres types d’applications, tels que les [applications monopages](./spa.md), [mobiles](./mobile.md) ou [Composants web](./web-component.md), utilisent généralement une autorisation, car il est difficile de sécuriser les informations d’identification.

Lorsque vous autorisez des requêtes à AEM as a Cloud Service, utilisez l’[authentification par jeton basée sur les informations d’identification du service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=fr). Pour en savoir plus sur l’authentification des requêtes à AEM as a Cloud Service, consultez la section [Tutoriel sur l’authentification par jeton](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=fr). Le tutoriel explore l’authentification par jeton à l’aide des [API HTTP AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=fr), mais les mêmes concepts et approches s’appliquent aux applications qui interagissent avec les API GraphQL AEM Headless.

## Exemple d’application serveur à serveur

Adobe fournit un exemple d’application serveur à serveur codée dans Node.js.

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="Application serveur à serveur" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="Application serveur à serveur">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="Application serveur à serveur">Application serveur à serveur</a></p>
                   <p class="is-size-6">Exemple d’application serveur à serveur, écrite dans Node.js, qui consomme du contenu des API GraphQL AEM Headless.</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Afficher l’exemple</span>
</a>
               </div>
           </div>
       </div>
    </div>
</div>
