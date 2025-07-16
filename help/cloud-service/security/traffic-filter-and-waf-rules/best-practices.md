---
title: Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF
description: Découvrez les bonnes pratiques recommandées pour la configuration des règles de filtrage du trafic, y compris les règles WAF dans AEM as a Cloud Service pour améliorer la sécurité et atténuer les risques.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 18%

---

# Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF

Découvrez les bonnes pratiques recommandées pour la configuration des règles de filtrage du trafic, y compris les règles WAF dans AEM as a Cloud Service pour améliorer la sécurité et atténuer les risques.

>[!IMPORTANT]
>
>Les bonnes pratiques décrites dans cet article ne sont pas exhaustives et ne sont pas destinées à se substituer à vos propres politiques et procédures de sécurité.

## Bonnes pratiques générales

- Commencez par l’[ensemble recommandé](./overview.md#adobe-recommended-rules) de règles standard de filtrage du trafic et de WAF fournies par Adobe, puis ajustez-les en fonction des besoins spécifiques et du paysage des menaces de votre application.
- Collaborez avec votre équipe de sécurité pour déterminer les règles alignées sur la position de sécurité et les exigences de conformité de votre entreprise.
- Testez toujours les règles nouvelles ou mises à jour dans les environnements de développement avant de les promouvoir dans les environnements d’évaluation et de production.
- Lors de la déclaration et de la validation des règles, commencez avec le type de `action` `log` pour observer le comportement sans bloquer le trafic légitime.
- Passez de `log` à `block` uniquement après avoir analysé suffisamment de données de trafic et confirmé qu’aucune requête valide n’est affectée.
- Introduisez progressivement des règles, en faisant intervenir des équipes de contrôle qualité, de performance et de test de sécurité pour identifier les effets secondaires inattendus.
- Examinez et analysez régulièrement l’efficacité des règles à l’aide de [outils de tableau de bord](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). La fréquence de la révision (quotidienne, hebdomadaire, mensuelle) doit correspondre au volume de trafic et au profil de risque de votre site.
- Affinez continuellement les règles en fonction des nouvelles informations sur les menaces, du comportement du trafic et des résultats d’audit.

## Bonnes pratiques relatives aux règles de filtrage du trafic

- Utilisez Adobe [règles de filtrage du trafic standard recommandées](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) comme ligne de base, qui inclut des règles pour les restrictions Edge, la protection d’origine et basées sur OFAC.
- Consultez régulièrement les alertes et les journaux pour identifier les schémas d’abus ou de mauvaise configuration.
- Ajustez les valeurs de seuil des limites de débit en fonction des modèles de trafic et du comportement des utilisateurs de votre application.

  Consultez le tableau suivant pour savoir comment choisir les valeurs de seuil :

  | Variante | Valeur |
  | :--------- | :------- |
  | Origine | Prenez la valeur la plus élevée du nombre maximum de requêtes à l’origine par adresse IP/PoP dans des conditions **normales** de trafic (c’est-à-dire, pas lors d’un DDoS) et multipliez-la. |
  | Edge | Prenez la valeur la plus élevée du nombre maximum de requêtes Edge par adresse IP/PoP dans des conditions **normales** de trafic (c’est-à-dire, pas lors d’un DDoS) et multipliez-la. |

  Consultez également la section [choix des valeurs de seuil](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) pour plus d’informations.

- Passez à `block` action uniquement après avoir confirmé que l’action `log` n’a aucune incidence sur le trafic légitime.

## Bonnes pratiques relatives aux règles WAF

- Commencez par les [règles de WAF recommandées](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) d’Adobe, qui incluent des règles pour bloquer les adresses IP erronées connues, détecter les attaques DDoS et limiter les abus de robots.
- L’indicateur `ATTACK` WAF doit vous avertir des menaces potentielles. Assurez-vous qu’il n’y a aucun faux positif avant de passer à `block`.
- Si les règles WAF recommandées ne couvrent pas des menaces spécifiques, pensez à créer des règles personnalisées en fonction des exigences uniques de votre application. Consultez une liste complète des [indicateurs WAF](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) dans la documentation.

## Modalités d&#39;application

Découvrez comment implémenter des règles de filtrage du trafic et des règles WAF dans AEM as a Cloud Service :

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF traffic filter rules}
  {description = Learn how to protect AEM websites from sophisticated threats including DoS, DDoS, and bot abuse using Adobe-recommended Web Application Firewall (WAF) traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-waf-rules.png}
  {cta = Activate WAF}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using standard traffic filter rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard">Protection des sites web AEM à l’aide de règles de filtrage du trafic standard</a>
                    </p>
                    <p class="is-size-6">Découvrez comment protéger les sites web AEM contre les attaques par déni de service, par déni de service et les abus de robots à l’aide des règles de filtrage du trafic standard recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Appliquer des règles</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Protection des sites web AEM à l’aide des règles de WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Protection des sites web AEM à l’aide des règles de WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protection des sites web AEM à l’aide des règles de WAF">Protection des sites web AEM à l’aide des règles de WAF</a>
                    </p>
                    <p class="is-size-6">Découvrez comment protéger les sites web d’AEM contre les menaces sophistiquées, y compris les attaques par déni de service, les attaques DDoS et les attaques de robots, à l’aide des règles de pare-feu d’application web (WAF) recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activer WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Ressources supplémentaires

- [Règles De Filtrage Du Trafic Incluant Les Règles WAF](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Comprendre la prévention des attaques par déni de service/déni de service dans AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Blocage des attaques DoS et DDoS à l’aide de règles de filtrage du trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)

