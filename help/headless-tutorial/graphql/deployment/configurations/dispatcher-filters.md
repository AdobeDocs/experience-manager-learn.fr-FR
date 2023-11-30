---
title: Filtres du Dispatcher pour GraphQL d’AEM
description: Découvrez comment configurer les filtres du Dispatcher de publication AEM à utiliser avec GraphQL d’AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 100%

---

# Filtres Dispatcher

Adobe Experience Manager as a Cloud Service utilise les filtres du Dispatcher de publication AEM pour s’assurer que seules les requêtes qui doivent atteindre AEM atteignent AEM. Par défaut, toutes les requêtes sont refusées et les modèles des URL autorisées doivent être explicitement ajoutés.

| Type de client | [Application monopage (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuration des filtres du Dispatcher requise | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.

## Configurer les filtres du Dispatcher

La configuration des filtres du Dispatcher de l’instance de publication AEM définit les modèles d’URL autorisés à atteindre AEM. Elle doit inclure le préfixe d’URL pour le point d’entrée de la requête persistante d’AEM.

| Le client se connecte à : | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Configuration des filtres du Dispatcher requise | ✘ | ✔ | ✔ |

Ajoutez une règle `allow` avec le modèle d’URL `/graphql/execute.json/*` et assurez-vous que l’ID de fichier (par exemple, `/0600`, est unique dans l’exemple de fichier de batterie).
Vous autorisez ainsi la requête HTTP GET au point d’entrée de la requête persistante, comme `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` vers l’instance de publication AEM.

Si vous utilisez des fragments d’expérience dans votre expérience AEM Headless, procédez de la même manière pour ces chemins d’accès.

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

+ [Vous trouverez un exemple de filtre du Dispatcher dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
