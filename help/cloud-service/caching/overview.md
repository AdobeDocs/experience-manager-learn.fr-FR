---
title: AEM mise en cache as a Cloud Service
description: Présentation générale de AEM mise en cache as a Cloud Service.
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 0%

---


# AEM mise en cache as a Cloud Service

Dans AEM as a Cloud Service, il est essentiel de comprendre la mise en cache. La mise en cache implique le stockage et la réutilisation des données récupérées précédemment afin d’améliorer l’efficacité du système et de réduire les temps de chargement. Ce mécanisme accélère considérablement la diffusion de contenu, améliore les performances du site web et optimise l’expérience utilisateur.

AEM as a Cloud Service comporte plusieurs calques de mise en cache et des stratégies qui diffèrent entre les services de création et de publication.

![Présentation de la mise en cache as a Cloud Service AEM](./assets/overview/all.png){align="center"}

## AEM de la mise en cache

AEM as a Cloud Service dispose d’une stratégie de mise en cache multi-couches robuste et configurable, notamment un réseau de diffusion de contenu, AEM Dispatcher et éventuellement un réseau de diffusion de contenu géré par le client. Il est possible d’affiner la mise en cache entre les calques afin d’optimiser les performances, en s’assurant qu’AEM offre uniquement les meilleures expériences. AEM a des préoccupations de mise en cache différentes pour les services de création et de publication. Explorez les stratégies de mise en cache pour chaque service ci-dessous.


<div class="columns is-multiline" style="margin-top: 2rem">
    <div class="column is-half-tablet is-half-desktop is-half-widescreen" aria-label="AEM Publish service caching">
    <div class="card is-padded-small is-padded-big-mobile" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="./publish.md" title="Service de publication d’AEM" tabindex="-1">
              <img class="is-bordered-r-small" src="./assets/overview/publish-card.png" alt="Mise en cache du service de publication AEM">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="./publish.md" title="Mise en cache du service de publication AEM">Mise en cache du service de publication AEM</a></p>
            <p class="is-size-6">AEM service de publication utilise un réseau de diffusion de contenu géré et AEM Dispatcher pour optimiser les expériences web des utilisateurs finaux.</p>
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
                <a href="./author.md" title="Mise en cache du service d’auteur AEM" tabindex="-1">
                <img class="is-bordered-r-small" src="./assets/overview/author-card.png" alt="Mise en cache du service d’auteur AEM">
                </a>
            </figure>
            </div>
            <div class="card-content is-padded-small">
            <div class="content">
                <p class="headline is-size-6 has-text-weight-bold"><a href="./author.md" title="Mise en cache du service d’auteur AEM">Mise en cache du service d’auteur AEM</a></p>
                <p class="is-size-6">AEM service de création utilise un réseau de diffusion de contenu géré pour fournir des expériences de création optimisées.</p>
                <a href="./author.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Apprendre</span>
</a>
            </div>
            </div>
        </div>
    </div>
</div>
