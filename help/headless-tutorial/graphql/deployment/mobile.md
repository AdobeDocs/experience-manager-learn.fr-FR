---
title: AEM les déploiements mobiles sans affichage
description: Découvrez les considérations relatives au déploiement pour les déploiements sans affichage AEM mobile.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# AEM les déploiements mobiles sans affichage

AEM les déploiements mobiles sans affichage sont des applications mobiles natives pour iOS, Android™, etc. qui consomment et interagissent avec le contenu d’AEM sans interface utilisateur graphique.

Les déploiements mobiles nécessitent une configuration minimale, car les connexions HTTP à AEM API sans affichage ne sont pas initiées dans le contexte d’un navigateur.

## Configurations de déploiement

La configuration de déploiement suivante doit être statique pour les déploiements d’applications mobiles.

| L’application mobile se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Partage de ressources cross-origin (CORS) | ✘ | ✘ | ✘ |
| [Hôtes AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemples d’applications mobiles

Adobe fournit des exemples d’applications mobiles iOS et Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="application iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="application iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="application iOS">application iOS</a></p>
                   <p class="is-size-6">Exemple d’application iOS, écrite dans SwiftUI, qui consomme du contenu des API GraphQL AEM sans affichage.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemple de vue</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Application Android™" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Application Android">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Application Android™">Application Android™</a></p>
                   <p class="is-size-6">Exemple d’application Java™ Android™ qui consomme du contenu des API GraphQL AEM sans affichage.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemple de vue</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>


