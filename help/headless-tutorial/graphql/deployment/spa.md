---
title: Déploiement de SPA pour AEM GraphQL
description: Découvrez les points à prendre en compte pour le déploiement d’applications d’une seule page (SPA) AEM déploiements sans affichage.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# AEM déploiements sans affichage SPA


AEM déploiements d’applications monopages sans affichage (SPA) impliquent des applications JavaScript conçues à l’aide de structures telles que React, Vue ou Angular, qui consomment et interagissent avec le contenu d’une manière sans interface utilisateur de typeheadless.

Le déploiement d’un SPA qui interagit AEM sans interface utilisateur nécessite l’hébergement du fichier et son accessibilité par le biais d’un navigateur web.

## Hébergez le SPA

Un SPA est constitué d’un ensemble de ressources web natives : **HTML, CSS et JavaScript**. Ces ressources sont générées au cours de la _build_ process (par exemple, `npm run build`) et déployés sur un hôte pour être utilisés par les utilisateurs finaux.

Il existe plusieurs **hébergement** options en fonction des exigences de votre entreprise :

1. **Fournisseurs cloud** par exemple **Azure** ou **AWS**.

2. **On-premise** hébergement dans une entreprise **centre de données**

3. **Plateformes d’hébergement front-end** par exemple **AWS Amplifier**, **Azure App Service**, **Netlify**, **Heroku**, **Vercel**, etc.

## Configurations de déploiement

La principale considération lors de l’hébergement d’un SPA qui interagit avec AEM sans interface utilisateur, est si l’accès à la base de données est effectué par l’intermédiaire d’un domaine (ou d’un hôte)  ou sur un autre domaine.  La raison en est SPA que les applications web s’exécutent dans des navigateurs web et sont donc soumises aux stratégies de sécurité des navigateurs web.

### Domaine partagé

Un SPA et un AEM partagent des domaines lorsque les utilisateurs finaux accèdent tous deux à partir du même domaine. Par exemple :

+ AEM est accessible via : `https://wknd.site/`
+ SPA est accessible via `https://wknd.site/spa`

Comme les accès à AEM et aux SPA sont effectués à partir du même domaine, les navigateurs web permettent à la version de l’ de définir XHR sur des points de terminaison sans affichage, sans avoir à utiliser de norme CORS, et permettent le partage de cookies HTTP (tels que les cookies). `login-token` (cookie).

La manière dont le trafic SPA et AEM est acheminé sur le domaine partagé dépend de vous : CDN avec plusieurs origines, serveur HTTP avec proxy inverse, hébergeant le SPA directement dans AEM, etc.

Vous trouverez ci-dessous les configurations de déploiement requises pour SPA déploiements en production, lorsqu’elles sont hébergées sur le même domaine qu’AEM.

| SPA se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| Partage de ressources cross-origin (CORS) | ✘ | ✘ | ✘ |
| Hôtes AEM | ✘ | ✘ | ✘ |

### Différents domaines

Un SPA et un AEM ont des domaines différents lorsqu’ils sont accessibles par des utilisateurs finaux provenant de différents domaines. Par exemple :

+ AEM est accessible via : `https://wknd.site/`
+ SPA est accessible via `https://wknd-app.site/`

Comme les accès à AEM et aux SPA sont effectués à partir de différents domaines, les navigateurs Web appliquent des stratégies de sécurité telles que [partage de ressources cross-origin (CORS)](./configurations/cors.md)et empêcher le partage de cookies HTTP (tels que AEM `login-token` (cookie).

Vous trouverez ci-dessous les configurations de déploiement requises pour SPA déploiements en production, lorsqu’elles sont hébergées sur un domaine différent d’AEM.

| SPA se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Filtres Dispatcher](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [Partage de ressources cross-origin (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [Hôtes AEM](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### Exemple de déploiement SPA sur différents domaines

Dans cet exemple, la SPA est déployée sur un domaine Netlify (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) et la SPA utilise AEM API GraphQL du domaine de publication AEM (`https://publish-p65804-e666805.adobeaemcloud.com`). Les captures d’écran ci-dessous mettent en évidence les exigences CORS.

1. Le SPA est diffusé à partir d’un domaine Netlify, mais effectue un appel XHR pour AEM les API GraphQL sur un autre domaine. Cette requête intersite nécessite [CORS](./configurations/cors.md) à configurer sur AEM pour permettre à la demande du domaine Netlify d’accéder à son contenu.

   ![Requête SPA diffusée à partir des hôtes SPA et AEM ](assets/spa/cors-requirement.png)

2. Inspection de la requête XHR vers l’API AEM GraphQL, la variable `Access-Control-Allow-Origin` est présent, indiquant au navigateur web qu’AEM autorise les requêtes de ce domaine Netlify à accéder à son contenu.

   Si l’AEM [CORS](./configurations/cors.md) était absent ou n’incluait pas le domaine Netlify, le navigateur web échouait à la requête XHR et signalait une erreur CORS.

   ![API d’en-tête de réponse CORS AEM GraphQL](assets/spa/cors-response-headers.png)

## Exemple d’application d’une seule page

Adobe fournit un exemple d’application monopage codée dans React.

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="application React" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="application React">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="application React">application React</a></p>
               <p class="is-size-6">Exemple d’application d’une seule page, écrite dans React, qui consomme du contenu des API GraphQL AEM sans affichage.</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Exemple de vue</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
