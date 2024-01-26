---
title: Déploiements d’AEM Headless
description: Découvrez les différents points à prendre en compte pour le déploiement des applications AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
exl-id: 6de58ca0-9444-4272-9487-15a9e3c89231
duration: 136
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 100%

---

# Déploiements d’AEM Headless

Les déploiements de clients d’AEM Headless peuvent prendre plusieurs formes : SPA hébergée par AEM, SPA externe, site web, application mobile, ou même processus de serveur à serveur.

En fonction du client et de la manière dont il est déployé, les déploiements d’AEM Headless présentent différents points à prendre en compte.

## Architecture de service AEM

Avant d’explorer les points à prendre en compte pour le déploiement, il est impératif de comprendre l’architecture logique d’AEM, ainsi que la séparation et les rôles des niveaux de service d’AEM as a Cloud Service. AEM as a Cloud Service comprend deux services logiques :

+ Le service de __Création AEM__ permet aux équipes de créer, collaborer et publier des fragments de contenu (et d’autres ressources).
+ Le service de __Publication AEM__ permet aux fragments de contenu publiés (et aux autres ressources) d’être répliqués pour la consommation générale.
+ Le service de __Prévisualisation AEM__ imite le service de Publication AEM sur le plan du comportement, mais son contenu est publié à des fins de prévisualisation ou de révision. Le service de Prévisualisation AEM est destiné à un public interne, et non à la diffusion générale de contenu. L’utilisation du service de Prévisualisation AEM est facultative, et dépend du workflow souhaité.

![Architecture de service AEM.](./assets/overview/aem-service-architecture.png)

Architecture classique d’un déploiement découplé d’AEM as a Cloud Service :

Les clients et clientes AEM Headless opérant dans le cadre d’une capacité de production interagissent généralement avec le service de Publication AEM, qui contient le contenu approuvé et publié. Les clients et clientes qui interagissent avec la service de Création AEM doivent être particulièrement vigilants, car le service de Création AEM est sécurisé par défaut et nécessite une autorisation pour toutes les requêtes, et peut également contenir des travaux en cours, ou du contenu non approuvé.

## Déploiements de clients découplés

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Application web monopage (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Applications web monopages (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Application web monopage (SPA)">Application web monopage (SPA)</a></p>
                   <p class="is-size-6">Découvrez les considérations relatives au déploiement des applications web monopages (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
               </div>
           </div>
       </div>
    </div>
<!-- Web component/JS -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web component/JS" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./web-component.md" title="Composant Web/JS" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/web-component/web-component-card.png" alt="Composant Web/JS">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./web-component.md" title="Composant Web/JS">Composant Web/JS</a></p>
               <p class="is-size-6">Découvrez les points à prendre en compte pour le déploiement des composants web et des consommateurs et consommatrices JavaScript découplés basés sur le navigateur.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
           </div>
       </div>
   </div>
</div>
<!-- Mobile apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./mobile.md" title="Applications mobiles" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/mobile/mobile-card.png" alt="Applications mobiles">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./mobile.md" title="Applications mobiles">Application mobile</a></p>
               <p class="is-size-6">Découvrez les points à prendre en compte pour le déploiement des applications mobiles.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
           </div>
       </div>
   </div>
</div>
<!-- Server-to-server apps -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server apps" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="./server-to-server.md" title="Applications serveur à serveur" tabindex="-1">
                   <img class="is-bordered-r-small" src="./assets/server-to-server/server-to-server-card.png" alt="Applications serveur à serveur">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="./server-to-server.md" title="Applications serveur à serveur">Application serveur à serveur</a></p>
               <p class="is-size-6">Découvrez les points à prendre en compte pour le déploiement des applications de serveur à serveur.</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
           </div>
       </div>
   </div>
</div>
</div>
