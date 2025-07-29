---
title: Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF
description: Découvrez les bonnes pratiques recommandées pour la configuration des règles de filtrage du trafic, y compris les règles WAF, dans AEM as a Cloud Service pour renforcer la sécurité et limiter les risques.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18310
thumbnail: null
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
source-git-commit: 71454ea9f1302d8d1c08c99e937afefeda2b1322
workflow-type: ht
source-wordcount: '638'
ht-degree: 100%

---

# Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF

Découvrez les bonnes pratiques recommandées pour la configuration des règles de filtrage du trafic, y compris les règles WAF, dans AEM as a Cloud Service pour renforcer la sécurité et limiter les risques.

>[!IMPORTANT]
>
>Les bonnes pratiques décrites dans le présent article ne sont pas exhaustives et n’ont pas pour but de se substituer à vos propres politiques et procédures de sécurité.

## Bonnes pratiques générales

- Commencez par l’[ensemble recommandé](./overview.md#adobe-recommended-rules) de règles WAF et standard de filtrage du trafic fournies par Adobe, puis ajustez-les en fonction des besoins spécifiques et du paysage des menaces de votre application.
- Collaborez avec votre équipe de sécurité pour déterminer quelles règles correspondent à la posture de sécurité de votre organisation et à ses exigences en matière de conformité.
- Testez toujours les règles nouvelles ou mises à jour dans les environnements de développement avant de les promouvoir dans les environnements d’évaluation et de production.
- Lors de la déclaration et de la validation des règles, commencez avec le type `action` `log` pour observer le comportement sans bloquer le trafic légitime.
- Passez de `log` à `block` uniquement après avoir analysé suffisamment de données de trafic et confirmé qu’aucune requête valide n’est affectée.
- Introduisez progressivement des règles, en faisant intervenir des équipes de contrôle qualité, de performances et de test de sécurité pour identifier les effets secondaires inattendus.
- Examinez et analysez régulièrement l’efficacité des règles à l’aide des [outils de tableau de bord](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). La fréquence de la révision (quotidienne, hebdomadaire, mensuelle) doit correspondre au volume de trafic et au profil de risque de votre site.
- Affinez continuellement les règles en fonction des nouvelles informations sur les menaces, du comportement du trafic et des résultats d’audit.

## Bonnes pratiques relatives aux règles de filtrage du trafic

- Utilisez comme référence les [règles de filtrage du trafic standard recommandées](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-starter-rules) par Adobe, qui incluent des règles pour la protection en périphérie (edge), à l’origine et des restrictions de l’OFAC.
- Consultez régulièrement les alertes et les journaux pour identifier les modèles d’abus ou de mauvaise configuration.
- Ajustez les valeurs de seuil des limites de débit en fonction des modèles de trafic et du comportement des utilisateurs et utilisatrices de votre application.

  Consultez le tableau suivant pour savoir comment choisir les valeurs de seuil :

  | Variante | Valeur |
  | :--------- | :------- |
  | Origine | Prenez la valeur la plus élevée du nombre maximum de requêtes à l’origine par adresse IP/PoP dans des conditions **normales** de trafic (c’est-à-dire, pas lors d’un DDoS) et multipliez-la. |
  | Edge | Prenez la valeur la plus élevée du nombre maximum de requêtes Edge par adresse IP/PoP dans des conditions **normales** de trafic (c’est-à-dire, pas lors d’un DDoS) et multipliez-la. |

  Consultez également la section [Choix des valeurs de seuil](../blocking-dos-attack-using-traffic-filter-rules.md#choosing-threshold-values) pour plus d’informations.

- Passez à l’action `block` uniquement après avoir confirmé que l’action `log` n’a aucune incidence sur le trafic légitime.

## Bonnes pratiques relatives aux règles WAF

- Commencez par les [règles WAF recommandées](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#recommended-nonwaf-starter-rules) par Adobe, qui incluent des règles pour bloquer les adresses IP erronées connues, détecter les attaques DDoS et limiter les abus de robots.
- L’indicateur WAF `ATTACK` doit vous avertir des menaces potentielles. Assurez-vous qu’il n’y a aucun faux positif avant de passer à `block`.
- Si les règles WAF recommandées ne couvrent pas des menaces spécifiques, pensez à créer des règles personnalisées en fonction des exigences uniques de votre application. Consultez une liste complète des [indicateurs WAF](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#waf-flags-list) dans la documentation.

## Implémentation des règles

Découvrez comment implémenter des règles de filtrage du trafic et des règles WAF dans AEM as a Cloud Service :

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
                    <a href="./use-cases/using-traffic-filter-rules.md" title="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-traffic-filter-rules.png" alt="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" title="Protection des sites web AEM à l’aide de règles de filtrage du trafic standard">Protection des sites web AEM à l’aide de règles de filtrage du trafic standard</a>
                    </p>
                    <p class="is-size-6">Découvrez comment protéger les sites web AEM contre les attaques DoS, DDoS et les abus de robots à l’aide des règles de filtrage de trafic standard recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-traffic-filter-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Appliquer les règles</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Protecting AEM websites using WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/using-waf-rules.md" title="Protection des sites web AEM à l’aide des règles WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/using-waf-rules.png" alt="Protection des sites web AEM à l’aide des règles WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" title="Protection des sites web AEM à l’aide des règles WAF">Protection des sites web AEM à l’aide des règles WAF</a>
                    </p>
                    <p class="is-size-6">Découvrez comment protéger les sites web AEM contre les menaces sophistiquées, y compris les attaques Dos, DDoS et les abus de robots, à l’aide des règles WAF (Web Application Firewall) recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activer WAF</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Ressources supplémentaires

- [Règles de filtrage de trafic incluant des règles WAF](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
- [Comprendre la prévention des attaques DoS/DDoS dans AEM](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/foundation/security/understanding-dos-and-prevention-approaches)
- [Bloquer des attaques DoS et DDoS à l’aide de règles de filtrage de trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/blocking-dos-attack-using-traffic-filter-rules)
