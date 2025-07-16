---
title: Normalisation des requêtes
description: Découvrez comment normaliser les requêtes en les transformant à l’aide de règles de filtrage du trafic dans AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# Normalisation des requêtes

Découvrez comment normaliser les requêtes en les transformant à l’aide de règles de filtrage du trafic dans AEM as a Cloud Service.

## Pourquoi et quand transformer des requêtes

Les transformations de requête sont utiles lorsque vous souhaitez normaliser le trafic entrant et réduire les écarts inutiles causés par des paramètres de requête ou des en-têtes inutiles. Cette technique est généralement utilisée pour :

- Améliorez l’efficacité de la mise en cache en supprimant les paramètres de contournement de la mémoire cache qui ne sont pas pertinents pour l’application AEM.
- Protégez l’origine des abus en réduisant les permutations de requête et en limitant les traitements inutiles.
- Assainissez ou simplifiez les requêtes avant de les transférer à AEM.

Ces transformations sont généralement appliquées au niveau de la couche du réseau CDN, en particulier pour les niveaux de publication AEM qui diffusent le trafic public.

## Prérequis

Avant de poursuivre, assurez-vous d’avoir terminé la configuration requise, comme décrit dans le tutoriel [Comment configurer le filtre de trafic et les règles de WAF](../setup.md). En outre, vous avez cloné et déployé le [projet AEM WKND Sites](https://github.com/adobe/aem-guides-wknd) dans votre environnement AEM.

## Exemple : annulation de la définition de paramètres de requête non nécessaires à l’application

Dans cet exemple, vous configurez une règle qui **supprime tous les paramètres de requête** à l’exception de `search` et `campaignId` pour réduire la fragmentation du cache.

- Ajoutez la règle suivante au fichier de projet WKND `/config/cdn.yaml`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- Validez et envoyez les modifications au référentiel Git de Cloud Manager.

- Déployez les modifications dans l’environnement AEM à l’aide du pipeline de configuration Cloud Manager [créé précédemment](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Testez la règle en accédant à la page du site WKND, par exemple `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- Dans les journaux AEM (`aemrequest.log`), vous devriez voir que la requête est transformée en `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`, avec le `otherParam` supprimé.

  ![Transformation de requête WKND](../assets/how-to/aemrequest-log-transformation.png)

