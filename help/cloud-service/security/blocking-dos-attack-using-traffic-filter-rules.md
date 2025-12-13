---
title: Blocage des attaques DoS, DDoS et sophistiquées à l’aide de règles de filtrage du trafic
description: Découvrez comment bloquer des attaques DoS, DDoS et sophistiquées à l’aide de règles de filtrage du trafic dans AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 100%

---

# Blocage des attaques DoS, DDoS et sophistiquées à l’aide de règles de filtrage du trafic

Découvrez comment bloquer des attaques par déni de service (DoS), par déni de service distribué (DDoS) et sophistiquées à l’aide de **règles de filtrage du trafic** sur le réseau CDN géré par AEM as a Cloud Service (AEMCS).

Ces attaques provoquent des pics de trafic sur le réseau de diffusion de contenu et potentiellement sur le service de publication AEM (soit l’origine) et peuvent avoir un impact sur la réactivité et la disponibilité du site.

Cet article présente les protections par défaut de votre site web AEM et explique comment étendre ces protections grâce à la configuration du client ou de la cliente. Il décrit également comment analyser les modèles de trafic et configurer des règles de filtrage de trafic standard pour bloquer ces attaques.

## Protections par défaut dans AEM as a Cloud Service

Examinons les protections par défaut contre les attaques DDoS de votre site web AEM :

- **Mise en cache :** avec de bonnes politiques de mise en cache, l’impact d’une attaque DDoS est plus limité, car le réseau de diffusion de contenu empêche la plupart des demandes d’accéder au point d’origine et d’entraîner une dégradation des performances.
- **Mise à l’échelle automatique :** les services de création et de publication d’AEM adaptent automatiquement leur échelle pour gérer les pics de trafic, mais des augmentations soudaines et massives du trafic peuvent quand même les affecter.
- **Blocage :** le réseau de diffusion de contenu Adobe bloque le trafic vers le point d’origine s’il dépasse un débit défini par Adobe à partir d’une adresse IP spécifique, par PoP (Point de présence) du réseau de diffusion de contenu.
- **Alertes :** le Centre d’actions envoie une notification d’alerte de pic de trafic au point d’origine lorsque le trafic dépasse un certain débit. Cette alerte se déclenche lorsque le trafic sur un réseau de diffusion de contenu donné dépasse une valeur de taux de requêtes par adresse IP _définie par Adobe_. Voir [Alertes des règles de filtrage de trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) pour plus d’informations.

Ces protections intégrées doivent être considérées comme une référence pour la capacité d’une organisation à minimiser l’impact d’une attaque DDoS sur les performances. Chaque site web ayant des caractéristiques de performances différentes et pouvant ainsi subir une dégradation des performances avant que la limite de débit définie par Adobe ne soit atteinte, il est recommandé d’étendre les protections par défaut via la _configuration des clientes et clients_.

## Extension de la protection avec des règles de filtrage du trafic

Voici quelques autres mesures recommandées que les clientes et clients peuvent prendre pour protéger leurs sites web contre les attaques DDoS :

- Implémentez les [règles de filtrage du trafic standard](./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md) recommandées par Adobe pour identifier les modèles de trafic potentiellement malveillants en consignant et en alertant sur les comportements suspects.
- Utilisez le module complémentaire **Protection WAF-DDoS** ou **Sécurité renforcée** et implémentez les [règles de filtrage du trafic WAF](./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md) recommandées par Adobe pour vous défendre contre les attaques sophistiquées, y compris celles qui utilisent un protocole avancé ou des techniques basées sur le payload.
- Augmentez la couverture du cache en configurant des [transformations de requêtes](./traffic-filter-and-waf-rules/how-to/request-transformation.md) pour ignorer des paramètres de requête inutiles.

## Commencer

Consultez les tutoriels suivants pour configurer les règles recommandées par Adobe afin de bloquer les attaques.

<!-- CARDS
{target = _self}

* ./traffic-filter-and-waf-rules/setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./traffic-filter-and-waf-rules/assets/setup/rules-setup.png}
  {cta = Start Now}

* ./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/setup.md" title="Configurer des règles de filtrage du trafic, y compris des règles WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/setup/rules-setup.png" alt="Configurer des règles de filtrage du trafic, y compris des règles WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" title="Configurer des règles de filtrage du trafic, y compris des règles WAF">Configurer des règles de filtrage du trafic, y compris des règles WAF</a>
                    </p>
                    <p class="is-size-6">Découvrez comment créer, déployer et tester des règles de filtrage du trafic, y compris des règles WAF, puis analyser les résultats.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Démarrer maintenant</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" title="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-traffic-filter-rules.png" alt="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard">Protection des sites web AEM à l’aide de règles de filtrage du trafic standard</a>
                    </p>
                    <p class="is-size-6">Découvrez comment protéger les sites web AEM contre les attaques DoS, DDoS et les abus de robots à l’aide des règles de filtrage de trafic standard recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Appliquer les règles</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" title="Protection des sites web AEM à l’aide des règles de filtrage du trafic WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./traffic-filter-and-waf-rules/assets/use-cases/using-waf-rules.png" alt="Protection des sites web AEM à l’aide des règles de filtrage du trafic WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protection des sites web AEM à l’aide des règles de filtrage du trafic WAF">Protection des sites web AEM à l’aide des règles de filtrage du trafic WAF</a>
                    </p>
                    <p class="is-size-6">Découvrez comment protéger les sites web AEM contre les menaces sophistiquées, y compris les attaques Dos, DDoS et les abus de robots, à l’aide des règles de filtrage du trafic WAF (Web Application Firewall) recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./traffic-filter-and-waf-rules/use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activer le pare-feu WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
