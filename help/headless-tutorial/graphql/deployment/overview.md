---
title: AEM Déploiements sans affichage
description: Découvrez les différents points à prendre en compte pour le déploiement des applications AEM sans affichage.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10794
thumbnail: kt-10794.jpg
last-substantial-update: 2022-08-26T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---


# AEM Déploiements sans affichage

AEM les déploiements de clients sans affichage prennent de nombreuses formes ; SPA hébergé AEM, site Web, application mobile ou même processus serveur à serveur.

En fonction du client et de la manière dont il est déployé, AEM déploiements sans affichage ont des considérations différentes.

## Architecture AEM service

Avant d’explorer les considérations liées au déploiement, il est impératif de comprendre AEM architecture logique, ainsi que la séparation et les rôles des niveaux de service d’AEM as a Cloud Service. AEM as a Cloud Service comprend deux services logiques :

+ __Auteur AEM__ est le service vers lequel les équipes créent, collaborent et publient des fragments de contenu (et d’autres ressources).
+ __Publication AEM__ est le service qui a été publié ; les fragments de contenu (et d’autres ressources) sont répliqués pour une utilisation générale.
+ __Aperçu AEM__ est le service qui imite AEM Publish dans le comportement, mais qui contient du contenu publié à des fins de prévisualisation ou de révision. AEM Aperçu est destiné aux audiences internes et non à la diffusion générale de contenu. L’utilisation de l’aperçu AEM est facultative, selon le workflow souhaité.

![Architecture AEM service](./assets/overview/aem-service-architecture.png)

Architecture de déploiement as a Cloud Service sans tête standard AEM

AEM clients sans affichage opérant dans une capacité de production interagissent généralement avec AEM Publish, qui contient le contenu approuvé et publié. Les clients qui interagissent avec l’auteur AEM doivent faire preuve d’une attention particulière, car l’auteur AEM est sécurisé par défaut, ce qui nécessite une autorisation pour toutes les requêtes. Il peut également contenir du travail en cours ou du contenu non approuvé.

## Déploiements de clients sans affichage

<div class="columns is-multiline">
    <!-- Single-page App (SPA) -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single-page App (SPA)" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="./spa.md" title="Application d’une seule page (SPA)" tabindex="-1">
                       <img class="is-bordered-r-small" src="./assets/spa/spa-card.png" alt="Applications d’une seule page (SPA)">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="./spa.md" title="Application d’une seule page (SPA)">Application d’une seule page (SPA)</a></p>
                   <p class="is-size-6">Découvrez les considérations relatives au déploiement pour les applications d’une seule page (SPA).</p>
                   <a href="./spa.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
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
               <p class="is-size-6">Découvrez les points à prendre en compte concernant le déploiement des composants web et des clients JavaScript sans interface utilisateur de navigateur.</p>
               <a href="./web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
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
               <p class="is-size-6">Découvrez les considérations relatives au déploiement pour les applications mobiles.</p>
               <a href="./mobile.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
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
               <p class="is-size-6">En savoir plus sur les considérations de déploiement pour les applications serveur à serveur</p>
               <a href="./server-to-server.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>