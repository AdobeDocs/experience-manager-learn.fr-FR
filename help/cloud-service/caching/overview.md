---
title: Cache d’AEM as a Cloud Service
description: Présentation du cache d’AEM as a Cloud Service.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: e76ed4c5-3220-4274-a315-a75e549f8b40
duration: 71
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 100%

---

# Cache d’AEM as a Cloud Service

Dans AEM as a Cloud Service, il est essentiel de comprendre le cache. Le cache implique le stockage et la réutilisation des données récupérées précédemment afin d’améliorer l’efficacité du système et de réduire les temps de chargement. Ce mécanisme accélère considérablement la diffusion de contenu, améliore les performances du site web et optimise l’expérience client.

AEM as a Cloud Service comporte plusieurs couches de cache et des stratégies qui diffèrent entre les services de création et de publication.

![Vue d’ensemble du cache d’AEM as a Cloud Service.](./assets/overview/all.png){align="center"}

## Cache AEM

AEM as a Cloud Service dispose d’une stratégie de cache multi-couche robuste et configurable, comprenant un réseau CDN, le Dispatcher d’AEM et éventuellement un réseau CDN géré côté client. Il est possible d’affiner les couches du cache afin d’optimiser les performances, en s’assurant qu’AEM offre uniquement les meilleures expériences. AEM a des préoccupations différentes concernant le cache pour les services de création et de publication. Explorez les stratégies de cache pour chaque service ci-dessous.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Service de publication AEM" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Mise en cache du service de publication d’AEM">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Mise en cache du service de publication d’AEM">Mise en cache du service de publication d’AEM</a></p>
            <p class="is-size-6">Le service de publication AEM utilise un réseau CDN géré et le Dispatcher d’AEM pour optimiser les expériences web des personnes finales.</p>
            <a href="./publish.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
          </div>
        </div>
      </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Author service caching">
        <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
            <div class="card-image">
            <figure class="image is-16by9">
                <a href="./author.md" title="Cache du service de création AEM" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Cache du service de création AEM">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Cache du service de création AEM">Cache du service de création AEM</a></p>
                <p class="is-size-6">Le service de création AEM utilise un réseau CDN géré pour fournir des expériences de création optimisées.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
            </div>
            </div>
        </div>
    </div>
</div>
