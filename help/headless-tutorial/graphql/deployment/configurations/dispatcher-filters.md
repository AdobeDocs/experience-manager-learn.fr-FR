---
title: Filtres de Dispatcher pour AEM GraphQL
description: Découvrez comment configurer les filtres du Dispatcher de publication AEM à utiliser avec AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

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

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### Exemple de configuration de filtres

+ [Vous trouverez un exemple du filtre Dispatcher dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)