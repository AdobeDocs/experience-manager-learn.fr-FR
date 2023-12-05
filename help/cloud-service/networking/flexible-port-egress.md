---
title: Sortie de port flexible
description: Découvrez comment configurer et utiliser la sortie de port flexible pour prendre en charge les connexions externes d’AEM as a Cloud Service à des services externes.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 946
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 97%

---

# Sortie de port flexible

Découvrez comment configurer et utiliser les sorties de port flexibles pour prendre en charge les connexions externes d’AEM as a Cloud Service à des services externes.

## En quoi consistent les sorties de port flexibles ?

Une sortie de port flexible permet l’association de règles de transfert de port personnalisées et spécifiques à AEM as a Cloud Service, ce qui rend possible l’établissement de connexions entre AEM et des services externes.

Un programme Cloud Manager ne peut avoir qu’un type d’infrastructure réseau __unique__. Assurez-vous que l’adresse IP de sortie dédiée est le type d’infrastructure réseau le plus [approprié](./advanced-networking.md) pour votre environnement AEM as a Cloud Service avant d’exécuter les commandes suivantes.

>[!MORELIKETHIS]
>
> Consultez la [documentation sur la configuration réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#flexible-port-egress) pour AEM as a Cloud Service pour obtenir plus d’informations sur la sortie de port flexible.

## Conditions préalables

Les éléments suivants sont requis lors de la configuration d’une sortie de port flexible :

+ Projet Adobe Developer Console avec l’API Cloud Manager activée et les [autorisations de la personne propriétaire de l’entreprise Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/).
+ Accès aux [Informations d’authentification de l’API Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/).
   + ID d’organisation (ou ID d’organisation IMS)
   + ID client (ou clé API)
   + Jeton d’accès (ou jeton porteur)
+ ID de programme Cloud Manager
+ ID d’environnement de Cloud Manager

Pour plus d’informations, regardez la présentation suivante pour découvrir comment installer, configurer et obtenir les informations d’identification de l’API Cloud Manager, et comment les utiliser pour effectuer un appel d’API Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Ce tutoriel utilise `curl` pour créer les configurations de l’API Cloud Manager. Les commandes `curl` fournies reposent sur la syntaxe Linux/macOS. Si vous utilisez l’invite de commande Windows, remplacez le caractère saut de ligne `\` par `^`.

## Activer la sortie de port flexible par programme

Commencez par activer la sortie de port flexible sur AEM as a Cloud Service.

1. Tout d’abord, déterminez la région dans laquelle le réseau avancé est configuré à l’aide de l’opération [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. L’élément `region name` est nécessaire pour effectuer les appels ultérieurs à l’API Cloud Manager. En règle générale, la région dans laquelle l’environnement de production réside est utilisée.

   Recherchez la région de votre environnement AEM as a Cloud Service AEM dans [Cloud Manager](https://my.cloudmanager.adobe.com) sous les [détails de l’environnement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=fr#viewing-environment). Le nom de région affiché dans Cloud Manager peut être [mappé au code de région](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) utilisé dans l’API Cloud Manager.

   __Requête HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Activez la sortie de port flexible pour un programme Cloud Manager à l’aide de l’opération [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. Utilisez le code `region` approprié obtenu suite à l’opération `listRegions` de l’API Cloud Manager.

   __Requête HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Patientez 15 minutes pendant que le programme Cloud Manager approvisionne l’infrastructure réseau.

1. Vérifiez que l’environnement a terminé la configuration de la __sortie de port flexible__ à l’aide de l’opération [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) de l’API Cloud Manager, en utilisant l’`id` renvoyé par la requête HTTP createNetworkInfrastructure de l’étape précédente.

   __Requête HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Vérifiez que la réponse HTTP renvoie le __statut__ __prêt__. Le cas échéant, vérifiez le statut après quelques minutes.

## Configurer les proxys de sortie de port flexible par environnement

1. Activez et configurez la configuration de __sortie de port flexible__ sur chaque environnement AEM as a Cloud Service à l’aide de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager.

   __Requête HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Définissez les paramètres JSON d’un objet `flexible-port-egress.json` fourni à cURL via l’objet `... -d @./flexible-port-egress.json`.

   [Téléchargez l’exemple d’objet flexible-port-egress.json](./assets/flexible-port-egress.json). Ce fichier n’est qu’un exemple. Configurez votre fichier selon les besoins en fonction des champs facultatifs/obligatoires documentés dans [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   Pour chaque mappage `portForwards`, la mise en réseau avancée définit la règle de transfert suivante :

   | Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si votre déploiement AEM nécessite __uniquement__ des connexions HTTP/HTTPS (port 80/443) vers un service externe, laissez le tableau `portForwards` vide, car ces règles ne s’appliquent qu’aux requêtes autres que HTTP/HTTPS.

1. Pour chaque environnement, vérifiez que les règles de sortie sont en vigueur à l’aide de l’opération [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager.

   __Requête HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Les configurations de sortie de port flexible peuvent être mises à jour à l’aide de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. Rappelez-vous que `enableEnvironmentAdvancedNetworkingConfiguration` est une opération `PUT`, donc toutes les règles doivent donc être fournies à chaque appel de cette opération.

1. Vous pouvez désormais utiliser la configuration de sortie de port flexible dans votre code AEM personnalisé et dans votre configuration.


## Se connecter à des services externes via une sortie de port flexible

Lorsque le proxy de sortie de port flexible est activé, le code AEM et la configuration peuvent les utiliser pour effectuer des appels vers des services externes. Il existe deux types d’appels externes qu’AEM traite différemment :

1. Appels HTTP/HTTPS aux services externes sur les ports non standard
   + Cela inclut les appels HTTP/HTTPS effectués aux services s’exécutant sur des ports autres que les ports 80 ou 443 standard.
1. Appels non HTTP/HTTPS aux services externes
   + Cela inclut tous les appels non HTTP, tels que les connexions aux serveurs de messagerie, aux bases de données SQL ou aux services qui s’exécutent sur d’autres protocoles non HTTP/HTTPS.

Les requêtes HTTP/HTTPS provenant d’AEM sur les ports standard (80/443) sont autorisées par défaut et ne nécessitent aucune configuration ni considération supplémentaires.


### HTTP/HTTPS sur les ports non standard

Lors de la création de connexions HTTP/HTTPS vers des ports non standard (autres que 80/443) à partir d’AEM, les connexions doivent être établies par le biais d’hôtes et de ports spéciaux, fournis via des espaces réservés.

AEM fournit deux jeux de variables système Java™ spéciales qui correspondent à des proxys HTTP/HTTPS d’AEM.

| Nom de variable | Utilisez | Code Java™ | Configuration OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Hôte proxy pour les connexions HTTP/HTTPS. | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | Port proxy pour les connexions HTTPS (définissez la version de secours sur `3128`). | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | Port proxy pour les connexions HTTPS (définissez la version de secours sur `3128`). | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Lors d’appels HTTP/HTTPS à des services externes sur des ports non standard, aucun `portForwards` correspondant ne doit être défini à l’aide de l’opération `enableEnvironmentAdvancedNetworkingConfiguration` de l’API Cloud Manager, car les « règles » de transfert de port sont définies « dans le code ».

>[!TIP]
>
> Consultez la documentation sur la sortie de port flexible d’AEM as a Cloud Service pour connaître [l’ensemble des règles de routage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#flexible-port-egress-traffic-routing).

#### Exemples de code

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS sur les ports non standard" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS sur les ports non standard</a></strong></div>
    <p>
        Exemple de code Java™ permettant de réaliser une connexion HTTP/HTTPS d’AEM as a Cloud Service à un service externe sur des ports HTTP/HTTPS non standard.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Connexions non HTTP/HTTPS à des services externes

Lors de la création de connexions non HTTP/HTTPS (par exemple, SQL, SMTP, etc.) depuis AEM, la connexion doit être établie par un nom d’hôte spécial fourni par AEM.

| Nom de variable | Utilisez | Code Java™ | Configuration OSGi |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Hôte proxy pour les connexions non HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


Les connexions à des services externes sont ensuite appelées par l’intermédiaire de `AEM_PROXY_HOST` et du port mappé (`portForwards.portOrig`), qu’AEM achemine ensuite vers le nom d’hôte externe mappé (`portForwards.name`) et le port (`portForwards.portDest`).

| Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemples de code

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connexion SQL à l’aide de JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connexion SQL à l’aide de JDBC DataSourcePool</a></strong></div>
      <p>
            Exemple de code Java™ se connectant à des bases de données SQL externes en configurant le pool de la source de données JDBC d’AEM.
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
      <div><strong><a href="./examples/email-service.md">Service e-mail</a></strong></div>
      <p>
        Exemple de configuration OSGi utilisant AEM pour se connecter à des services de messagerie externes.
      </p>
    </td>   
</tr></table>
