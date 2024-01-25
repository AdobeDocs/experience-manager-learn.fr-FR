---
title: Déploiements de composants web AEM Headless
description: Découvrez les points à prendre en compte pour les déploiements AEM Headless basés sur des composants web et JS purs.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 54
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 100%

---

# Déploiements de composants web AEM Headless

Les déploiements AEM Headless des [composants web](https://developer.mozilla.org/fr-FR/docs/Web/Web_Components) et JS sont des applications JavaScript pures qui s’exécutent dans un navigateur web, qui consomment et interagissent avec le contenu dans AEM de manière découplée. Les déploiements de composants web et JS diffèrent des [déploiements SPA](./spa.md), car ils n’utilisent pas un cadre SPA robuste, et sont censés être intégrés dans le contexte de tout site web et diffuser en surface le contenu d’AEM.


## Configurations de déploiement

La configuration de déploiement suivante doit être en place pour les déploiements de composants web et JS.

| L’application composant web et JS se connecte à | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher.](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Partage de ressources entre origines multiples (CORS).](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hôtes AEM.](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemple de composant web

Adobe fournit un exemple de composant web.

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Composant web" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Composant web">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Composant web">Composant web</a></p>
                   <p class="is-size-6">Un exemple de composant web, écrit en JavaScript pur, qui consomme le contenu des API GraphQL d’AEM Headless.</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Afficher l’exemple</span>
</a>
               </div>
           </div>
       </div>
    </div>
</div>
