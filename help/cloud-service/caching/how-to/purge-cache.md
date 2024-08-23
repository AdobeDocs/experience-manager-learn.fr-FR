---
title: Comment purger le cache CDN
description: Découvrez comment purger ou supprimer la réponse HTTP mise en cache du réseau de diffusion de contenu AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
source-git-commit: e03480e37463b7d35a49203e331ce661b6dc52f9
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 2%

---

# Comment purger le cache CDN

Découvrez comment purger ou supprimer la réponse HTTP mise en cache du réseau de diffusion de contenu AEM as a Cloud Service. À l’aide de la fonction en libre-service appelée **Purge du jeton API**, vous pouvez purger le cache d’une ressource spécifique, d’un groupe de ressources et de l’ensemble du cache.

Dans ce tutoriel, vous apprenez à configurer et à utiliser le jeton API de purge pour purger le cache CDN de l’exemple de site [AEM WKND](https://github.com/adobe/aem-guides-wknd) à l’aide de la fonctionnalité de libre-service.

>[!VIDEO](https://video.tv.adobe.com/v/3432948?quality=12&learn=on)

## Invalidation du cache par rapport à la purge explicite

Il existe deux façons de supprimer les ressources mises en cache du réseau de diffusion de contenu :

1. **Invalidation du cache :** Il s’agit du processus de suppression des ressources mises en cache du CDN en fonction des en-têtes de cache tels que `Cache-Control`, `Surrogate-Control` ou `Expires`. La valeur d’attribut `max-age` de l’en-tête du cache est utilisée pour déterminer la durée de vie du cache des ressources, également appelée délai d’activation du cache. À l’expiration de la durée de vie du cache, les ressources mises en cache sont automatiquement supprimées du cache CDN.

1. **Purge explicite :** Il s’agit du processus de suppression manuelle des ressources mises en cache du cache CDN avant l’expiration du délai d’activation. La purge explicite est utile lorsque vous souhaitez supprimer immédiatement les ressources mises en cache. Cependant, cela augmente le trafic vers le serveur d’origine.

Lorsque des ressources mises en cache sont supprimées du cache CDN, la requête suivante pour la même ressource récupère la dernière version du serveur d’origine.

## Configuration du jeton API de purge

Découvrez comment configurer le jeton API de purge pour purger le cache CDN.

### Configuration de la règle CDN

Le jeton API de purge est créé en configurant la règle CDN dans le code de votre projet AEM.

1. Ouvrez le fichier `cdn.yaml` du dossier `config` principal de votre projet AEM. Par exemple, le fichier [cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) du projet WKND.

1. Ajoutez la règle CDN suivante au fichier `cdn.yaml` :

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

1. Enregistrez, validez et poussez les modifications dans le référentiel Adobe en amont.

### Création d’une variable d’environnement Cloud Manager

Créez ensuite les variables d’environnement Cloud Manager pour stocker la valeur Purge API Token .

1. Connectez-vous à Cloud Manager à l’adresse [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) et sélectionnez votre organisation et votre programme.

1. Dans la section __Environments__ (Environnements), cliquez sur les **ellipses** (...) en regard de l’environnement souhaité, puis sélectionnez **Afficher les détails**.

   ![Afficher les détails](../assets/how-to/view-env-details.png)

1. Sélectionnez ensuite l’onglet **Configuration** et cliquez sur le bouton **Ajouter la configuration** .

1. Dans la boîte de dialogue **Configuration de l’environnement**, saisissez les détails suivants :
   - **Name** : saisissez le nom de la variable d’environnement. Elle doit correspondre à la valeur `purgeKey1` ou `purgeKey2` du fichier `cdn.yaml`.
   - **Valeur** : entrez la valeur du jeton API de purge.
   - **Service appliqué** : sélectionnez l’option **Tous** .
   - **Type** : sélectionnez l’option **Secret** .
   - Cliquez sur le bouton **Ajouter**.

   ![Ajouter une variable](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. Répétez les étapes ci-dessus pour créer la seconde variable d’environnement pour la valeur `purgeKey2`.

1. Cliquez sur **Enregistrer** pour enregistrer et appliquer les modifications.

### Déploiement de la règle CDN

Enfin, déployez la règle CDN configurée sur l’environnement AEM as a Cloud Service à l’aide du pipeline Cloud Manager.

1. Dans Cloud Manager, accédez à la section **Pipelines** .

1. Créez un nouveau pipeline ou sélectionnez le pipeline existant qui déploie uniquement les fichiers **Config**. Pour obtenir des instructions détaillées, voir [Création d’un pipeline de configuration](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Cliquez sur le bouton **Exécuter** pour déployer la règle CDN.

   ![Exécuter le pipeline](../assets/how-to/run-config-pipeline.png)

## Utilisation du jeton API de purge

Pour purger le cache CDN, appelez l’URL de domaine spécifique au service AEM avec le jeton API de purge. La syntaxe pour purger le cache est la suivante :

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

Où :

- **PURGE`<URL>`** : la méthode `PURGE` est suivie du chemin URL de la ressource que vous souhaitez purger.
- **Hôte :`<AEM_SERVICE_SPECIFIC_DOMAIN>`** : il spécifie le domaine du service AEM.
- **X-AEM-Purge-Key :`<PURGE_API_TOKEN>`** : un en-tête personnalisé qui contient la valeur du jeton API de purge.
- **X-AEM-Purge :`<PURGE_TYPE>`** : un en-tête personnalisé qui spécifie le type d’opération de purge. La valeur peut être `hard`, `soft` ou `all`. Le tableau suivant décrit chaque type de purge :

  | Type de purge | Description |
  |:------------:|:-------------:|
  | hard (par défaut) | Supprime immédiatement la ressource mise en cache. Évitez-le, car cela augmente le trafic vers le serveur d’origine. |
  | soft | Marque la ressource mise en cache comme obsolète et récupère la dernière version du serveur d’origine. |
  | tout | Supprime toutes les ressources mises en cache du cache CDN. |

- **Surrogate-Key :`<SURROGATE_KEY>`** : (facultatif) en-tête personnalisé qui spécifie les clés de substitution (séparées par un espace) des groupes de ressources à purger. La clé de substitution est utilisée pour regrouper les ressources et doit être définie dans l’en-tête de réponse de la ressource.

>[!TIP]
>
>Dans les exemples ci-dessous, le `X-AEM-Purge: hard` est utilisé à des fins de démonstration. Vous pouvez le remplacer par `soft` ou `all` en fonction de vos besoins. Soyez prudent lorsque vous utilisez le type de purge `hard`, car cela augmente le trafic vers le serveur d’origine.

### Purge du cache d’une ressource spécifique

Dans cet exemple, la commande `curl` purge le cache de la ressource `/us/en.html` sur le site WKND déployé dans un environnement AEM as a Cloud Service.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

Une fois la purge terminée, une réponse `200 OK` est renvoyée avec du contenu JSON.

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### Purge du cache d’un groupe de ressources

Dans cet exemple, la commande `curl` purge le cache du groupe de ressources avec la clé de substitution `wknd-assets`. L’en-tête de réponse `Surrogate-Key` est défini dans [`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176), par exemple :

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

Une fois la purge terminée, une réponse `200 OK` est renvoyée avec du contenu JSON.

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### Purge de l’intégralité du cache

Dans cet exemple, à l’aide de la commande `curl`, l’intégralité du cache est purgée de l’exemple de site WKND déployé sur l’environnement AEM as a Cloud Service.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

Une fois la purge terminée, une réponse `200 OK` est renvoyée avec du contenu JSON.

```json
{"status":"ok"}
```

### Vérification de la purge du cache

Pour vérifier la purge du cache, accédez à l’URL de la ressource dans le navigateur web et passez en revue les en-têtes de réponse. La valeur de l’en-tête `X-Cache` doit être `MISS`.

![En-tête X-Cache](../assets/how-to/x-cache-miss.png)