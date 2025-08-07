---
title: Présentation de la personnalisation
description: Découvrez comment personnaliser des sites web AEM as a Cloud Service à l’aide d’applications Adobe Target et Adobe Experience Platform.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 6%

---

# Présentation de la personnalisation

Découvrez comment AEM as a Cloud Service (AEMCS) s’intègre à Adobe Target et Adobe Experience Platform (AEP). Découvrez comment offrir des expériences personnalisées à l’aide de tests A/B, en ciblant les utilisateurs en fonction de leur comportement ou en personnalisant le contenu à l’aide de profils client.

## Prérequis

Pour démontrer divers scénarios de personnalisation, ce tutoriel utilise l’exemple de projet [AEM WKND](https://github.com/adobe/aem-guides-wknd/). Pour suivre, vous avez besoin de :

- Une organisation Adobe ayant accès à :
   - **environnement AEM as a Cloud Service** - pour créer et gérer du contenu
   - **Adobe Target** - pour composer et diffuser des expériences personnalisées
   - **Applications Adobe Experience Platform** - pour gérer les profils et audiences de clients
   - **Balises (anciennement Launch) dans AEP** - pour déployer le SDK Web et le JavaScript personnalisé pour la collecte de données et la personnalisation

- Compréhension de base des composants AEM et des fragments d’expérience

- Le projet [AEM WKND](https://github.com/adobe/aem-guides-wknd/) déployé dans votre environnement AEM as a Cloud Service.

## Commencer

Avant d’explorer des cas d’utilisation spécifiques, vous devez d’abord configurer AEM as a Cloud Service pour la personnalisation. Commencez par intégrer Adobe Target et les balises pour activer la personnalisation côté client à l’aide de la SDK web AEP. Ces étapes fondamentales permettent à vos pages AEM de prendre en charge l’expérimentation, le ciblage des audiences et la personnalisation en temps réel.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content as Adobe Target offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Intégrer Adobe Target" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Intégrer Adobe Target"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Intégrer Adobe Target">Intégrer Adobe Target</a>
                    </p>
                    <p class="is-size-6">Intégrez AEMCS à Adobe Target pour activer le contenu personnalisé en tant qu’offres Adobe Target.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Intégrer Target</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="Intégration de balises" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="Intégration de balises"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="Intégration de balises">Intégrer des balises</a>
                    </p>
                    <p class="is-size-6">Intégrez AEM CS aux balises pour injecter le SDK web et le JavaScript personnalisé pour la collecte de données et la personnalisation.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Intégrer des balises</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## Cas d’utilisation

Explorez les cas d’utilisation courants de la personnalisation ci-dessous pris en charge par AEMCS, Adobe Target et Adobe Experience Platform.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
  {title = Experimentation (A/B Testing)}
  {description = Learn how to test different content variations in AEMCS using Adobe Target for A/B testing.}
  {image = ./assets/use-cases/experiment/experimentation.png}
  {cta = Learn Experimentation}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Expérimentation (Test A/B)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Expérimentation (Test A/B)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Expérimentation (Test A/B)">Expérimentation (Tests A/B)</a>
                    </p>
                    <p class="is-size-6">Découvrez comment tester différentes variations de contenu dans AEM CS à l’aide d’Adobe Target pour les tests A/B.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre l’expérimentation</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



















