---
title: Surveillance des utilisateurs réels (RUM)
description: Découvrez la surveillance des utilisateurs réels (RUM) sur AEM site web as a Cloud Service.
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# Surveillance des utilisateurs réels (RUM)

Découvrez la surveillance des utilisateurs réels (RUM) sur AEM site web as a Cloud Service. Découvrez comment activer RUM, quelles données sont collectées et comment utiliser les données RUM pour optimiser l’expérience utilisateur sur votre site web.

## Vue d’ensemble

Real User Monitoring (RUM) est une méthode utilisée pour _collecter, mesurer et analyser les interactions et les expériences des utilisateurs ;_ avec un site web en temps réel. Il fournit des informations sur la manière dont les visiteurs du site interagissent avec votre site web, notamment leur comportement, leurs performances et leur expérience globale. Pour ce faire, il vous suffit d’injecter une petite partie de code JavaScript dans les pages du site.

Grâce au code JavaScript, RUM capture les données directement à partir du navigateur de l’utilisateur lorsqu’il interagit avec votre site web. Ces données peuvent être utilisées pour identifier et diagnostiquer les problèmes de performances, optimiser l’expérience utilisateur et améliorer les résultats commerciaux.

La fonctionnalité RUM d’AEM as a Cloud Service offre une vue d’ensemble complète de l’expérience utilisateur de votre site web. Il capture les mesures clés suivantes pour chaque page (URL) visitée par l’utilisateur :

- [Peinture à contenu le plus grand (LCP)](https://web.dev/articles/lcp) : mesure les performances de chargement.
- [Maj de mise en page cumulée (CLS)](https://web.dev/articles/cls) : mesure la stabilité visuelle.
- [Interaction avec la prochaine peinture (INP)](https://web.dev/articles/inp) - mesure l’interactivité.
- Pages vues : mesure le nombre de fois où une page est consultée.

Il capture également les erreurs 404 et les graphiques des pages vues du site web.

Les mesures LCP, CLS et INP font partie des [Principales Vitesses Web](https://web.dev/articles/vitals) qui sont un ensemble de mesures liées à la vitesse, à la réactivité et à la stabilité visuelle d’un site web. Ces mesures sont utilisées par Google pour mesurer l’expérience utilisateur sur un site web et sont importantes pour le classement des moteurs de recherche.

## Activer RUM

Pour activer RUM pour votre site web AEM, voir [Configuration du service de surveillance des utilisateurs réel](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

Les détails clés de RUM dans AEMCS sont les suivants :

- Le RUM s’applique uniquement au service de publication d’AEM, ce qui signifie que le code JavaScript est injecté uniquement dans l’environnement de publication.
- La variable `com.adobe.granite.webvitals.WebVitalsConfig` La configuration OSGi contrôle les chemins d’inclusion et d’exclusion, il s’agit de chemins de référentiel et non de chemins d’URL.
- Par défaut `/content` path est inclus.
- Pour exclure des chemins, ajoutez le `AEM_WEBVITALS_EXCLUDE` Variable d’environnement Cloud Manager, voir [Ajout de variables d’environnement](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). Les chemins sont séparés par une virgule.
- Le code OOTB est chargé d’injecter le code JavaScript dans les pages.

### Vérification

Pour vérifier si RUM est activé pour votre site web, consultez la source de HTML de la page publiée et recherchez les blocs de script suivants :

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## Collecte de données RUM

- Les données RUM sont collectées à l’aide de `sampleRUM()` en transmettant le nom du point de contrôle. Dans l’exemple ci-dessus, les points de contrôle sont les suivants : `top`, `load`, `click`, `lazy`, et `cwv`.
- Un point de contrôle est un événement nommé dans la séquence de chargement de la page et d’interaction avec celle-ci.

Voir aussi [Real User Monitoring Service et confidentialité](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) et [Données en cours de collecte](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) pour plus d’informations.

## Vue des données RUM

Pour afficher les données RUM dont vous avez besoin `domainkey`, Adobe le fournit dans le cadre de la configuration RUM. Le tableau de bord RUM est disponible à l’adresse [https://data.aem.live/](https://data.aem.live/) et vous pouvez y accéder à l’aide de la clé de domaine et de l’URL .

Par exemple, la capture d’écran ci-dessous montre le tableau de bord RUM pour le site web AEM WKND.

![Tableau de bord RUM](./assets/rum/RUM-Dashboard-WKND.png)

Le tableau de bord RUM fournit les informations clés suivantes :

- **Mesures de performances** - LCP, CLS, INP et Pageviews.
- **Mesures d’erreur** - Erreurs 404.
- **Graphiques Pageview** - Nombre de pages vues au fil du temps.

## Utilisation des données RUM

En utilisant les informations ci-dessus, vous pouvez optimiser l’expérience utilisateur sur votre site web. Par exemple :

- Réduire les performances LCP, CLS et INP pour améliorer les performances de chargement des pages et l’interactivité. Voir [Comment améliorer LCP](https://web.dev/articles/lcp#improve-lcp), [Comment améliorer la ligne de commande](https://web.dev/articles/cls#improve-cls) et [Comment améliorer INP](https://web.dev/articles/inp#improve-inp)pour plus d’informations.
- Pour améliorer l’expérience utilisateur, corrigez les erreurs 404.
- Pour comprendre le comportement de l’utilisateur et optimiser le contenu, analysez les graphiques des pages vues.

Adobe recommande une révision régulière du tableau de bord RUM, notamment après une mise à jour majeure ou mineure.

Voir aussi [Qui peut bénéficier de Real User Monitoring Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) pour plus d’informations.
