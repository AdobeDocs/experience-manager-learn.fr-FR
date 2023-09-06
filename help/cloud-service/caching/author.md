---
title: Mise en cache du service d’auteur AEM
description: Présentation générale de la mise en cache du service de création as a Cloud Service AEM.
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 3%

---


# Création AEM

AEM Auteur dispose d’une mise en cache limitée en raison de la nature très dynamique et sensible aux autorisations du contenu qu’il diffuse. En règle générale, il n’est pas recommandé de personnaliser la mise en cache pour AEM Auteur et de plutôt s’appuyer sur les configurations de cache fournies par Adobe pour garantir une expérience performante.

![Diagramme de présentation de la mise en cache de l’auteur AEM](./assets/author/author-all.png){align="center"}

Bien que la personnalisation de la mise en cache sur AEM Author soit déconseillée, il est utile de comprendre qu’AEM Author dispose d’un réseau de diffusion de contenu géré par l’Adobe, mais pas d’un Dispatcher AEM. N’oubliez pas que toutes les configurations AEM Dispatcher sont ignorées sur AEM Author, car il ne dispose pas d’un Dispatcher.

## Réseau de diffusion de contenu

AEM service de création utilise un réseau de diffusion de contenu, mais son objectif est d’améliorer la diffusion des ressources de produit et ne doit pas être entièrement configuré, mais plutôt de le laisser fonctionner tel quel.

![Diagramme de présentation de la mise en cache AEM](./assets/author/author-cdn.png){align="center"}

Le réseau de diffusion de contenu d’auteur AEM se trouve entre l’utilisateur final, généralement un spécialiste du marketing ou un auteur de contenu, et l’auteur AEM. Il met en cache les fichiers non modifiables, tels que les ressources statiques qui alimentent l’expérience de création AEM et non le contenu créé.

Le réseau de diffusion de contenu de l’auteur d’AEM met en cache plusieurs types de ressources susceptibles d’être intéressantes, y compris une [TTL personnalisable sur les requêtes persistantes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances), et a [Durée de vie longue sur les bibliothèques clientes personnalisées](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### Durée de vie du cache par défaut

Les ressources suivantes destinées aux clients sont mises en cache par le réseau de diffusion de contenu de l’auteur AEM et ont la durée de vie par défaut suivante du cache :

| Type de contenu | Durée de vie du cache CDN par défaut |
|:------------ |:---------- |
| [Requêtes persistantes (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1 minute |
| [Bibliothèques clientes (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 jours |
| [Tout le reste](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | Non mis en cache |


## AEM Dispatcher

AEM service Auteur n’inclut pas AEM Dispatcher et utilise uniquement la variable [CDN](#cdn) pour la mise en cache.

