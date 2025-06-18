---
title: Déployer l’application SPA pour AEM GraphQL
description: Découvrez les points à prendre en compte pour le déploiement d’applications à page unique (SPA) AEM Headless.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 99%

---

# Déploiements de SPA AEM Headless

Les déploiements d’applications sur une seule page (SPA) AEM Headless impliquent des applications JavaScript conçues à l’aide de frameworks tels que React ou Vue, qui consultent et interagissent avec le contenu d’AEM de manière découplée.

Pour déployer une SPA qui interagit avec AEM de façon découplée, la SPA doit être hébergée et accessible dans un navigateur web.

## Héberger la SPA

Une SPA est constituée d’un ensemble de ressources web natives : **HTML, CSS et JavaScript**. Ces ressources sont générées au cours du processus de _création_ (par exemple, `npm run build`) et déployées sur un hôte pour leur consultation par les utilisateurs finaux et utilisatrices finales.

Plusieurs solutions d’**hébergement** sont proposées pour répondre aux besoins de votre entreprise :

1. **Fournisseurs cloud**, par exemple **Azure** ou **AWS**.

2. Hébergement **On-Premise** dans un **centre de données** d’entreprise.

3. **Plateformes d’hébergement front-end**, par exemple **AWS Amplify**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel**, etc.

## Configurations de déploiement

Le principal élément à prendre en compte lors de l’hébergement d’une SPA qui interagit avec AEM de manière découplée est de déterminer si l’accès à la SPA est réalisé via le domaine (ou l’hôte) AEM ou un autre domaine.  En effet, les SPA sont des applications web qui s’exécutent dans des navigateurs web et sont donc soumises aux politiques de sécurité de ceux-ci.

### Domaine partagé

Les domaines partagés de la SPA et d’AEM sont identiques si les utilisateurs finaux et utilisatrices finales y accèdent via le même domaine. Par exemple :

+ AEM est accessible via : `https://wknd.site/`
+ La SPA est accessible via : `https://wknd.site/spa`

Comme l’accès à AEM et à la SPA est réalisé via le même domaine, les navigateurs web permettent à la SPA d’envoyer des requêtes XHR aux points d’entréee AEM Headless, sans avoir à utiliser de configuration CORS. Ils autorisent également le partage de cookies HTTP (tel que le cookie `login-token` d’AEM).

Vous pouvez décider de la façon dont le trafic SPA et AEM est acheminé sur le domaine partagé : réseau CDN avec plusieurs origines, serveur HTTP avec proxy inverse, hébergement de la SPA directement dans AEM, etc.

Vous trouverez ci-dessous les configurations de déploiement requises pour le déploiement de la SPA dans l’instance de production, lorsqu’elles sont hébergées sur le même domaine qu’AEM.

| La SPA se connecte à → | Création AEM | Publication AEM | Prévisualisation AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher.](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Partage de ressources entre origines multiples (CORS) | ✘ | ✘ | ✘ |
| Hôtes AEM | ✘ | ✘ | ✘ |

### Différents domaines

Une SPA et AEM ont des domaines différents lorsque les utilisateurs finaux et utilisatrices finales y accèdent via des domaines différents. Par exemple :

+ AEM est accessible via : `https://wknd.site/`
+ La SPA est accessible via : `https://wknd-app.site/`

Comme l’accès à AEM et à la SPA est réalisé via des domaines différents, les navigateurs web appliquent des politiques de sécurité, telles que le [partage de ressources cross-origin (CORS)](./configurations/cors.md) et empêchent le partage de cookies HTTP (par exemple, le cookie `login-token` d’AEM).

Vous trouverez ci-dessous les configurations de déploiement requises pour le déploiement de la SPA dans l’instance de production, lorsqu’elles sont hébergées sur un domaine autre qu’AEM.

| La SPA se connecte à → | Création AEM | Publication AEM | Prévisualisation AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher.](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Partage de ressources entre origines multiples (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hôtes AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Exemple de déploiement d’une SPA sur différents domaines

Dans cet exemple, la SPA est déployée sur un domaine Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) et elle utilise les API AEM GraphQL du domaine de publication AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). Les captures d’écran ci-dessous mettent en évidence les exigences CORS.

1. La SPA est hébergée sur un domaine Netlify, mais effectue un appel XHR aux API GraphQL d’AEM sur un autre domaine. Cette requête multisite nécessite la configuration de la politique [CORS](./configurations/cors.md) sur AEM afin de permettre au domaine Netlify d’accéder à son contenu.

   ![Requête SPA à partir des hôtes SPA et AEM ](assets/spa/cors-requirement.png)

2. L’en-tête `Access-Control-Allow-Origin` est présent dans la requête XHR vers l’API AEM GraphQL, ce qui indique au navigateur web qu’AEM autorise les demandes provenant de ce domaine Netlify à accéder à son contenu.

   Si la politique [CORS](./configurations/cors.md) AEM est absente ou n’inclut pas le domaine Netlify, la requête XHR est refusée par le navigateur web qui indique une erreur CORS.

   ![En-tête de réponse CORS de l’API AEM GraphQL](assets/spa/cors-response-headers.png)

## Exemple d’application sur une seule page

Adobe fournit un exemple d’application sur une seule page codée dans React.

<!-- CARDS 

* ../example-apps/react-app.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React App - AEM Headless Example">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../example-apps/react-app.md" title="Application React - Exemple AEM Headless" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app.png" alt="Application React - Exemple AEM Headless"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../example-apps/react-app.md" target="_blank" rel="referrer" title="Application React - Exemple AEM Headless">Application React - Exemple AEM Headless</a>
                    </p>
                    <p class="is-size-6">Découvrez les fonctionnalités découplées d’Adobe Experience Manager (AEM) grâce aux exemples d’applications. Cette application React montre comment interroger le contenu à l’aide des API GraphQL d’AEM en utilisant des requêtes persistantes.</p>
                </div>
                <a href="../example-apps/react-app.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


