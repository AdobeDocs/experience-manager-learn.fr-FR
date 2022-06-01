---
title: Sortie de port flexible
description: Découvrez comment configurer et utiliser la sortie de port flexible pour prendre en charge les connexions externes d’AEM as a Cloud Service à des services externes.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 1%

---

# Sortie de port flexible

Découvrez comment configurer et utiliser la sortie de port flexible pour prendre en charge les connexions externes d’AEM as a Cloud Service à des services externes.

## Qu’est-ce que la sortie de port flexible ?

Une sortie de port flexible permet l’association de règles de transfert de port personnalisées et spécifiques à AEM as a Cloud Service, ce qui permet d’établir des connexions entre AEM et des services externes.

Un programme Cloud Manager ne peut avoir qu’une __single__ type d’infrastructure réseau. Assurez-vous que l’adresse IP sortante dédiée est la plus élevée [type approprié d’infrastructure réseau](./advanced-networking.md)  pour votre AEM as a Cloud Service avant d’exécuter les commandes suivantes.

>[!MORELIKETHIS]
>
> Lisez l’AEM as a Cloud Service [documentation sur la configuration réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) pour plus d’informations sur la sortie flexible du port.

## Prérequis

Les éléments suivants sont requis lors de la configuration d’une sortie de port flexible :

+ Projet de la console Adobe Developer avec l’API Cloud Manager activée et [Autorisations du propriétaire commercial Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Accès à [Informations d’identification d’authentification de l’API Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID d’organisation (alias ID d’organisation IMS)
   + ID client (alias clé API)
   + Jeton d’accès (ou jeton porteur)
+ ID de programme Cloud Manager
+ ID d’environnement de Cloud Manager

Pour plus d’informations, consultez la présentation suivante pour savoir comment configurer, configurer et obtenir les informations d’identification de l’API Cloud Manager, et comment les utiliser pour effectuer un appel de l’API Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

Ce tutoriel utilise `curl` pour créer les configurations de l’API Cloud Manager. Le `curl` supposent une syntaxe Linux/macOS. Si vous utilisez l’invite de commande Windows, remplacez la variable `\` caractère de saut de ligne avec `^`.

## Activation de la sortie de port flexible par programme

Commencez par activer la sortie flexible du port sur AEM as a Cloud Service.

1. Tout d’abord, déterminez la région dans laquelle la mise en réseau avancée sera configurée à l’aide de l’API Cloud Manager. [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) opération. Le `region name` est nécessaire pour effectuer les appels d’API Cloud Manager suivants. En règle générale, la région dans laquelle réside l’environnement de production est utilisée.

   __requête HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Activation de la récupération de port flexible pour un programme Cloud Manager à l’aide de l’API Cloud Manager [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) opération. Utilisez les `region` code obtenu à partir de l’API Cloud Manager `listRegions` opération.

   __requête HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Patientez 15 minutes pendant que le programme Cloud Manager approvisionne l’infrastructure réseau.

1. Vérifier que l&#39;environnement est terminé __sortie de port flexible__ configuration à l’aide de l’API Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) , à l’aide de la fonction `id` renvoyée par la requête HTTP createNetworkInfrastructure de l’étape précédente.

   __requête HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Vérifiez que la réponse HTTP contient une __status__ de __ready__. Si vous n’êtes pas encore prêt, vérifiez l’état toutes les quelques minutes.

## Configuration de proxys de sortie de port flexibles par environnement

1. Activez et configurez les __sortie de port flexible__ configuration sur chaque environnement as a Cloud Service AEM à l’aide de l’API Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) opération.

   __requête HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Définition des paramètres JSON dans une `flexible-port-egress.json` et fourni à curl via `... -d @./flexible-port-egress.json`.

   [Téléchargez l’exemple flexible-port-egress.json](./assets/flexible-port-egress.json). Ce fichier n’est qu’un exemple. Configurez votre fichier selon les besoins en fonction des champs facultatifs/obligatoires documentés à l’adresse [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   Pour chaque `portForwards` , la mise en réseau avancée définit la règle de transfert suivante :

   | Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si votre déploiement AEM __only__ nécessite des connexions HTTP/HTTPS (port 80/443) à un service externe, laissez la variable `portForwards` vide, car ces règles ne sont requises que pour les requêtes non HTTP/HTTPS.

1. Pour chaque environnement, validez les règles de sortie en vigueur à l’aide de l’API Cloud Manager. [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) opération.

   __requête HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Les configurations flexibles de sortie de port peuvent être mises à jour à l’aide de l’API Cloud Manager. [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) opération. Mémoriser `enableEnvironmentAdvancedNetworkingConfiguration` est un `PUT` toutes les règles doivent donc être fournies avec chaque appel de cette opération.

1. Vous pouvez désormais utiliser la configuration flexible de sortie de port dans votre code d’AEM personnalisé et dans votre configuration.


## Connexion à des services externes via une sortie de port flexible

Lorsque le proxy de sortie de port flexible est activé, le code AEM et la configuration peuvent les utiliser pour effectuer des appels vers des services externes. Il existe deux types d’appels externes qui AEM traitent différemment :

1. Appels HTTP/HTTPS aux services externes sur les ports non standard
   + Inclut les appels HTTP/HTTPS effectués aux services s’exécutant sur des ports autres que les ports 80 ou 443 standard.
1. Appels non HTTP/HTTPS aux services externes
   + Inclut tous les appels non HTTP, tels que les connexions aux serveurs de messagerie, aux bases de données SQL ou aux services qui s’exécutent sur d’autres protocoles non HTTP/HTTPS.

Les requêtes HTTP/HTTPS provenant d’AEM sur les ports standard (80/443) sont autorisées par défaut et ne nécessitent aucune configuration ni considérations supplémentaires.


### HTTP/HTTPS sur les ports non standard

Lors de la création de connexions HTTP/HTTPS à des ports non standard (non-80/443) à partir d’AEM, les connexions doivent être établies par le biais d’hôtes et de ports spéciaux, fournis via des espaces réservés.

AEM fournit deux ensembles de variables système Java™ spéciales qui correspondent à des proxys HTTP/HTTPS AEM.

| Nom de variable | Utilisation | Code Java™ | Configuration OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Hôte proxy pour les connexions HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | Port proxy pour les connexions HTTPS (définir la version de secours sur `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | Port proxy pour les connexions HTTPS (définir la version de secours sur `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Lors d’appels HTTP/HTTPS à des services externes sur des ports non standard, aucun appel correspondant `portForwards` doit être défini à l’aide de l’API Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , car les &quot;règles&quot; de transfert de port sont définies &quot;dans le code&quot;.

>[!TIP]
>
> Voir la documentation sur le port flexible d’AEM as a Cloud Service pour [l’ensemble complet des règles de routage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Exemples de code

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS sur les ports non standard" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS sur les ports non standard</a></strong></div>
    <p>
        Exemple de code Java™ qui rend la connexion HTTP/HTTPS d’AEM as a Cloud Service à un service externe sur des ports HTTP/HTTPS non standard.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Connexions non HTTP/HTTPS à des services externes

Lors de la création de connexions non HTTP/HTTPS (ex. SQL, SMTP, etc.) depuis AEM, la connexion doit être établie par un nom d’hôte spécial fourni par AEM.

| Nom de variable | Utilisation | Code Java™ | Configuration OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Hôte proxy pour les connexions non HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


Les connexions à des services externes sont ensuite appelées par l’intermédiaire de la fonction `AEM_PROXY_HOST` et le port mappé (`portForwards.portOrig`), qui AEM ensuite achemine vers le nom d’hôte externe mappé (`portForwards.name`) et port (`portForwards.portDest`).

| Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemples de code

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connexion SQL à l’aide de JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connexion SQL à l’aide de JDBC DataSourcePool</a></strong></div>
      <p>
            Exemple de code Java™ se connectant à des bases de données SQL externes en configurant AEM pool de sources de données JDBC.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Connexion SQL à l’aide des API Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Connexion SQL à l’aide des API Java™</a></strong></div>
      <p>
            Exemple de code Java™ se connectant à des bases de données SQL externes à l’aide des API SQL de Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Réseau privé virtuel (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Service de messagerie électronique</a></strong></div>
      <p>
        Exemple de configuration OSGi utilisant AEM pour se connecter à des services de messagerie externes.
      </p>
    </td>   
</tr></table>
