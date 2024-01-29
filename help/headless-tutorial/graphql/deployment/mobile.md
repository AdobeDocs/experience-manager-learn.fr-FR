---
title: Déploiements mobiles d’AEM Headless
description: Découvrez les considérations relatives aux déploiements mobiles d’AEM Headless.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10796
thumbnail: KT-10796.jpg
exl-id: 1f536079-b3ce-4807-be88-804378e75d37
duration: 75
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '161'
ht-degree: 100%

---

# Déploiements mobiles d’AEM Headless

Les déploiements mobiles d’AEM Headless sont des applications mobiles natives pour iOS, Android™, etc., qui consomment le contenu d’AEM de façon découplée et interagissent avec lui.

Les déploiements mobiles nécessitent une configuration minimale, car les connexions HTTP aux API d’AEM Headless ne sont pas initiées dans le contexte d’un navigateur.

## Configurations de déploiement

La configuration de déploiement suivante doit être mise en place pour les déploiements d’applications mobiles.

| L’application mobile se connecte à | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher.](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Partage de ressources entre origines multiples (CORS) | ✘ | ✘ | ✘ |
| [Hôtes AEM.](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Exemples d’applications mobiles

Adobe fournit des exemples d’applications mobiles iOS et Android™.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="Application iOS" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="Application iOS">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="Application iOS">Application iOS</a></p>
                   <p class="is-size-6">Exemple d’application iOS, écrite dans SwiftUI, qui consomme du contenu des API GraphQL d’AEM Headless.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Afficher l’exemple</span>
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
                   <p class="is-size-6">Exemple d’application Java™ Android™ qui consomme du contenu des API GraphQL d’AEM Headless.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Afficher l’exemple</span>
</a>
               </div>
           </div>
       </div>
    </div>
</div>
