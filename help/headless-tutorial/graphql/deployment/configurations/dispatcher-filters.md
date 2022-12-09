---
title: Filtres de Dispatcher pour GraphQL AEM
description: Découvrez comment configurer les filtres du Dispatcher de publication AEM à utiliser avec AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: 442020d854d8f42c5d8a1340afd907548875866e
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 2%

---


# Filtres Dispatcher

Adobe Experience Manager as a Cloud Service utilise les filtres du Dispatcher de publication AEM pour s’assurer que seules les requêtes qui doivent atteindre AEM atteignent AEM. Par défaut, toutes les demandes sont refusées et les modèles des URL autorisées doivent être explicitement ajoutés.

| Type de client | [Application d’une seule page (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuration des filtres Dispatcher requise | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.

## Configuration des filtres de Dispatcher

La configuration de filtre Dispatcher de publication AEM définit les modèles d’URL autorisés à atteindre AEM et doit inclure le préfixe d’URL pour le point de terminaison de requête persistant AEM.

| Le client se connecte à | Auteur AEM | Publication AEM | Aperçu AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Configuration des filtres Dispatcher requise | ✘ | ✔ | ✔ |

Ajoutez un `allow` règle avec le modèle d’URL `/graphql/execute.json/*`et assurez-vous que l’ID de fichier (par exemple : `/0600`, est unique dans l’exemple de fichier de ferme).
Cela permet la requête de GET HTTP au point de terminaison de requête persistant, comme `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` jusqu’à AEM Publish.

Si vous utilisez des fragments d’expérience dans votre expérience AEM sans affichage, procédez de la même manière pour ces chemins.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### Exemple de configuration de filtres

+ [Vous trouverez un exemple du filtre Dispatcher dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
