---
title: AEM Déploiements de composants web sans affichage
description: Découvrez les points à prendre en compte pour le déploiement des composants web/AEM JavaScript purs sans affichage.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# AEM Déploiements de composants web sans affichage

AEM sans tête [Composant Web](https://developer.mozilla.org/en-US/docs/Web/Web_Components)Les déploiements /JS sont des applications JavaScript pures qui s’exécutent dans un navigateur web, qui consomment et interagissent avec le contenu d’AEM sans interface utilisateur graphique. Les déploiements de composants Web/JS diffèrent de [Déploiements SPA](./spa.md) en ce sens qu’ils n’utilisent pas de structure SPA robuste et qu’ils sont censés être incorporés dans le contexte d’un site web, d’une diffusion, à un contenu superficiel provenant d’AEM.


## Configurations de déploiement

La configuration de déploiement suivante doit être en place pour les déploiements de composants Web/JS.

| Composant web/application JS se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Partage de ressources cross-origin (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hôtes AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemple de composant Web

Adobe fournit un exemple de composant Web.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Composant Web" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Composant Web">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Composant Web">Composant Web</a></p>
                   <p class="is-size-6">Un exemple de composant Web, écrit en JavaScript pur, qui consomme du contenu des API GraphQL AEM sans affichage.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemple de vue</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
