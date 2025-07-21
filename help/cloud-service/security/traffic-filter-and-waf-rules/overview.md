---
title: Présentation - Protection des sites web AEM
description: Découvrez comment protéger vos sites web AEM contre les attaques par déni de service, les attaques par déni de service et le trafic malveillant à l’aide des règles de filtrage du trafic, y compris sa sous-catégorie de règles de pare-feu d’application web (WAF) dans AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-13148
thumbnail: null
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: 22a35b008de380bf2f2ef5dfde6743261346df89
workflow-type: tm+mt
source-wordcount: '1185'
ht-degree: 1%

---

# Présentation - Protection des sites web AEM

Découvrez comment protéger vos sites web AEM contre les dénis de service (DoS), les dénis de service distribués (DDoS), le trafic malveillant et les attaques sophistiquées à l’aide de **règles de filtrage du trafic**, y compris sa sous-catégorie de règles **Pare-feu d’application web (WAF)** dans AEM as a Cloud Service.

Vous découvrirez également les différences entre les règles de filtrage du trafic standard et les règles de filtrage du trafic WAF, quand les utiliser et comment prendre en main les règles recommandées par Adobe.

>[!IMPORTANT]
>
> Les règles de filtrage du trafic WAF nécessitent une licence supplémentaire de **protection WAF-DDoS** ou **sécurité renforcée**. Les règles de filtrage du trafic standard sont disponibles par défaut pour les clients Sites et Forms.


>[!VIDEO](https://video.tv.adobe.com/v/3469394/?quality=12&learn=on)

## Présentation de la sécurité du trafic dans AEM as a Cloud Service

AEM as a Cloud Service utilise une couche de réseau CDN intégrée pour protéger et optimiser la diffusion de votre site web. L’un des composants les plus importants de la couche CDN est la possibilité de définir et d’appliquer des règles de trafic. Ces règles servent de bouclier de protection pour protéger votre site contre les abus, les utilisations abusives et les attaques, sans sacrifier les performances.

La sécurité du trafic est essentielle pour maintenir la disponibilité, protéger les données sensibles et assurer une expérience transparente pour les utilisateurs légitimes. AEM propose deux catégories de règles de sécurité :

- **Règles standard de filtrage du trafic**
- **Règles de filtrage du trafic du pare-feu d’application web (WAF)**

Les ensembles de règles aident les clients à prévenir les menaces Web courantes et sophistiquées, à réduire le bruit des clients malveillants ou inappropriés et à améliorer l&#39;observabilité grâce à la journalisation des demandes, au blocage et à la détection des motifs.

## Différence entre les règles de filtrage du trafic standard et WAF

| Fonctionnalité | Règles de filtrage de trafic standard | Règles de filtrage du trafic WAF |
|--------------------------|--------------------------------------------------|---------------------------------------------------------|
| Objectif | Prévenir les abus tels que les attaques par déni de service, les attaques par déni de service, le grattage ou les activités de robots | Détectez et réagissez aux schémas d’attaque sophistiqués (par exemple, le top 10 d’OWASP), qui vous protège également des robots. |
| Exemples | Limitation de débit, blocage géographique, filtrage agent-utilisateur | Injection SQL, XSS, adresses IP d’attaque connues |
| Flexibilité | Hautement configurable via YAML | Hautement configurable via YAML, avec des indicateurs WAF prédéfinis |
| Mode Recommandé | Commencez par le mode `log`, puis passez au mode `block` | Commencez par `block` mode pour `ATTACK-FROM-BAD-IP` indicateur WAF et `log` mode pour `ATTACK` indicateur WAF, puis passez en mode `block` pour les deux |
| Déploiement | Défini dans YAML et déployé via le pipeline de configuration Cloud Manager | Défini dans YAML avec `wafFlags` et déployé via le pipeline de configuration Cloud Manager |
| Octroi de licence | Inclus avec les licences Sites et Forms | **Nécessite une licence de protection WAF-DDoS ou de sécurité renforcée** |

Les règles de filtrage du trafic standard sont utiles pour appliquer des politiques spécifiques à l’entreprise, telles que des limites de débit ou le blocage de régions spécifiques, ainsi que pour bloquer le trafic en fonction des propriétés et des en-têtes de requête tels que l’adresse IP, le chemin d’accès ou l’agent utilisateur.
Les règles de filtrage du trafic de WAF, en revanche, offrent une protection proactive complète pour les exploits web et les vecteurs d’attaque connus, et disposent d’une intelligence avancée pour limiter les faux positifs (c’est-à-dire bloquer le trafic légitime).
Pour définir les deux types de règles, utilisez la syntaxe YAML. Pour plus d’informations, voir [Syntaxe des règles de filtrage de trafic](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#rules-syntax).

## Quand et pourquoi les utiliser

**Utilisez des règles de filtrage de trafic standard** lorsque :

- Vous souhaitez appliquer des limites spécifiques à une organisation, telles que le ralentissement du débit IP.
- Vous connaissez des modèles spécifiques (par exemple, adresses IP malveillantes, régions, en-têtes) qui nécessitent un filtrage.

**Utilisez les règles de filtrage du trafic WAF** lorsque :

- Vous souhaitez une protection complète **proactive** contre les schémas d’attaque connus et répandus (par exemple, les injections, les abus de protocole), ainsi que les adresses IP malveillantes connues, collectées auprès de sources de données expertes.
- Vous souhaitez refuser les requêtes malveillantes tout en limitant les risques de blocage du trafic légitime.
- Vous souhaitez limiter les efforts de défense contre des menaces courantes et sophistiquées en appliquant des règles de configuration simples.

Ensemble, ces règles fournissent une stratégie de défense en profondeur qui permet aux clients AEM as a Cloud Service de prendre des mesures à la fois proactives et réactives pour sécuriser leurs propriétés numériques.

## Règles recommandées par Adobe

Adobe fournit des règles recommandées pour le filtre de trafic standard et les règles de filtrage de trafic WAF afin de vous aider à sécuriser rapidement vos sites AEM.

- **Règles de filtrage du trafic standard** (disponible par défaut) : traitez les scénarios d’utilisation abusive courants tels que les attaques par déni de service, par déni de service et les attaques de robots contre les **périphériques de réseau CDN**, **origine** ou le trafic provenant de pays sanctionnés.\
  Voici quelques exemples :
   - Limitation du débit des adresses IP qui effectuent plus de 500 requêtes/seconde _à la périphérie du réseau CDN_
   - IP de limitation de débit qui effectuent plus de 100 requêtes/seconde _à l’origine_
   - Blocage du trafic provenant des pays répertoriés par l’Office of Foreign Assets Control (OFAC)

- **Règles de filtrage du trafic WAF** (nécessite une licence de module complémentaire) : fournit une protection supplémentaire contre les menaces sophistiquées, y compris les [principales menaces d’OWASP](https://owasp.org/www-project-top-ten/) telles que l’injection SQL, les scripts de site à site (XSS) et d’autres attaques d’applications web.
Voici quelques exemples :
   - Blocage des requêtes provenant d’adresses IP erronées connues
   - Journalisation ou blocage des requêtes suspectes signalées comme des attaques

>[!TIP]
>
> Commencez par appliquer les **règles recommandées par Adobe** pour bénéficier de l’expertise d’Adobe en matière de sécurité et des mises à jour continues. Si votre entreprise présente des risques spécifiques ou des cas de périphérie, ou remarque des faux positifs (blocage du trafic légitime), vous pouvez définir des **règles personnalisées** ou étendre le jeu par défaut pour répondre à vos besoins.

## Commencer

Découvrez comment définir, déployer, tester et analyser les règles de filtrage du trafic, y compris les règles WAF, dans AEM as a Cloud Service en suivant le guide de configuration et les cas d’utilisation ci-dessous. Vous disposez ainsi des connaissances de base vous permettant d’appliquer ultérieurement en toute confiance les règles recommandées par Adobe.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = How to set up traffic filter rules including WAF rules}
  {description = Learn how to set up to create, deploy, test, and analyze the results of traffic filter rules including WAF rules.}
  {image = ./assets/setup/rules-setup.png}
  {cta = Start Now}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="How to set up traffic filter rules including WAF rules">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="Configuration des règles de filtrage du trafic, y compris les règles WAF" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/rules-setup.png" alt="Configuration des règles de filtrage du trafic, y compris les règles WAF"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="Configuration des règles de filtrage du trafic, y compris les règles WAF">Configuration de règles de filtrage du trafic, y compris des règles WAF</a>
                    </p>
                    <p class="is-size-6">Découvrez comment configurer pour créer, déployer, tester et analyser les résultats des règles de filtrage de trafic, y compris les règles WAF.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Démarrer maintenant</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Guide de configuration des règles recommandé par Adobe

Ce guide fournit des instructions détaillées pour configurer et déployer les règles de filtrage du trafic standard et WAF recommandées par Adobe dans votre environnement AEM as a Cloud Service.

<!-- CARDS
{target = _self}

* ./use-cases/using-traffic-filter-rules.md
  {title = Protecting AEM websites using standard traffic filter rules}
  {description = Learn how to protect AEM websites from DoS, DDoS and bot abuse using Adobe-recommended standard traffic filter rules in AEM as a Cloud Service.}
  {image = ./assets/use-cases/using-traffic-filter-rules.png}
  {cta = Apply Rules}

* ./use-cases/using-waf-rules.md
  {title = Protecting AEM websites using WAF rules}
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
                    <p class="is-size-6">Découvrez comment protéger les sites web d’AEM contre les menaces sophistiquées, y compris les attaques par déni de service, les attaques DDoS et les attaques de robots, à l’aide des règles de filtrage du trafic WAF (Web Application Firewall) recommandées par Adobe dans AEM as a Cloud Service.</p>
                </div>
                <a href="./use-cases/using-waf-rules.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Activer WAF</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Cas d’utilisation avancés

Pour des scénarios plus avancés, vous pouvez explorer les cas d’utilisation suivants, qui montrent comment implémenter des règles de filtrage de trafic personnalisées en fonction d’exigences commerciales spécifiques :

<!-- CARDS
{target = _self}

* ./how-to/request-logging.md

* ./how-to/request-blocking.md

* ./how-to/request-transformation.md
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Monitoring sensitive requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-logging.md" title="Surveillance des requêtes sensibles" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/wknd-login.png" alt="Surveillance des requêtes sensibles"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-logging.md" target="_self" rel="referrer" title="Surveillance des requêtes sensibles">Surveillance des requêtes sensibles</a>
                    </p>
                    <p class="is-size-6">Découvrez comment surveiller les requêtes sensibles en les enregistrant à l’aide des règles de filtrage du trafic dans AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-logging.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Restricting access">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-blocking.md" title="Limitation de l’accès" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/elk-tool-dashboard-blocked.png" alt="Limitation de l’accès"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-blocking.md" target="_self" rel="referrer" title="Limitation de l’accès">Limitation de l’accès</a>
                    </p>
                    <p class="is-size-6">Découvrez comment restreindre l’accès en bloquant des requêtes spécifiques à l’aide des règles de filtrage du trafic dans AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-blocking.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Normalizing requests">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./how-to/request-transformation.md" title="Normalisation des requêtes" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="assets/how-to/aemrequest-log-transformation.png" alt="Normalisation des requêtes"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./how-to/request-transformation.md" target="_self" rel="referrer" title="Normalisation des requêtes">Normalisation des requêtes</a>
                    </p>
                    <p class="is-size-6">Découvrez comment normaliser les requêtes en les transformant à l’aide de règles de filtrage du trafic dans AEM as a Cloud Service.</p>
                </div>
                <a href="./how-to/request-transformation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus</span>
</a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Ressources supplémentaires

- [Règles De Filtrage Du Trafic Incluant Les Règles WAF](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)
