---
title: Adresse IP sortante dédiée
description: Découvrez comment configurer et utiliser une adresse IP de sortie dédiée, qui permet aux connexions sortantes d’AEM d’être issues d’une adresse IP dédiée.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '1186'
ht-degree: 0%

---


# Adresse IP sortante dédiée

Découvrez comment configurer et utiliser une adresse IP de sortie dédiée, qui permet aux connexions sortantes d’AEM d’être issues d’une adresse IP dédiée.

## Qu’est-ce que l’adresse IP de sortie dédiée ?

Une adresse IP sortante dédiée permet aux requêtes d’AEM as a Cloud Service d’utiliser une adresse IP dédiée, ce qui permet aux services externes de filtrer les requêtes entrantes par cette adresse IP. Comme [ports de sortie flexibles](./flexible-port-egress.md), une adresse IP sortante dédiée vous permet de passer sur des ports non standard.

Un programme Cloud Manager ne peut avoir qu’une __single__ type d’infrastructure réseau. Assurez-vous que l’adresse IP sortante dédiée est la plus élevée [type approprié d’infrastructure réseau](./advanced-networking.md)  pour votre AEM as a Cloud Service avant d’exécuter les commandes suivantes.

>[!MORELIKETHIS]
>
> Lisez l’AEM as a Cloud Service [documentation sur la configuration réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#dedicated-egress-IP-address) pour plus d’informations sur les adresses IP sortantes dédiées.

## Prérequis

Les éléments suivants sont requis lors de la configuration d’une adresse IP de sortie dédiée :

+ API Cloud Manager avec [Autorisations du propriétaire commercial Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Accès à [Informations d’identification de l’authentification de l’API Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID d’organisation (alias ID d’organisation IMS)
   + ID client (alias clé API)
   + Jeton d’accès (ou jeton porteur)
+ ID de programme Cloud Manager
+ ID d’environnement de Cloud Manager

Ce tutoriel utilise `curl` pour créer les configurations de l’API Cloud Manager. Le `curl` supposent une syntaxe Linux/macOS. Si vous utilisez l’invite de commande Windows, remplacez la variable `\` caractère de saut de ligne avec `^`.

## Activer l’adresse IP de sortie dédiée sur le programme

Commencez par activer et configurer l’adresse IP sortante dédiée sur AEM as a Cloud Service.

1. Tout d’abord, déterminez la région dans laquelle la mise en réseau avancée sera configurée à l’aide de l’API Cloud Manager. [listRegion](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) opération. Le `region name` sera nécessaire pour effectuer les appels d’API Cloud Manager suivants. En règle générale, la région dans laquelle réside l’environnement de production est utilisée.

   __requête HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Activation d’une adresse IP de sortie dédiée pour un programme Cloud Manager à l’aide de l’API Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) opération. Utilisez les `region` code obtenu à partir de l’API Cloud Manager `listRegions` opération.

   __requête HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Patientez 15 minutes pendant que le programme Cloud Manager approvisionne l’infrastructure réseau.

1. Vérifier que l&#39;environnement est terminé __adresse IP de sortie dédiée__ configuration à l’aide de l’API Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) , à l’aide de la fonction `id` renvoyée par la requête HTTP createNetworkInfrastructure de l’étape précédente.

   __requête HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Vérifiez que la réponse HTTP contient une __status__ de __ready__. Si vous n’êtes pas encore prêt, vérifiez l’état toutes les quelques minutes.

## Configuration de proxys d’adresses IP sortantes dédiés par environnement

1. Activez et configurez les __adresse IP de sortie dédiée__ configuration sur chaque environnement as a Cloud Service AEM à l’aide de l’API Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) opération.

   __requête HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Définition des paramètres JSON dans une `dedicated-egress-ip-address.json` et fourni à curl via `... -d @./dedicated-egress-ip-address.json`.

[Téléchargez l’exemple dedicated-egress-ip-address.json](./assets/dedicated-egress-ip-address.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   La signature HTTP de la configuration de l’adresse IP sortante dédiée diffère uniquement de [port de sortie flexible](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) en ce sens qu’il prend également en charge les options facultatives. `nonProxyHosts` configuration.

   `nonProxyHosts` déclare un ensemble d’hôtes pour lequel le port 80 ou 443 doit être acheminé par les plages d’adresses IP partagées par défaut plutôt que par l’adresse IP sortante dédiée. Cela peut s’avérer utile, car le trafic passant par les adresses IP partagées peut être optimisé automatiquement par Adobe.

   Pour chaque `portForwards` , la mise en réseau avancée définit la règle de transfert suivante :

   | Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Pour chaque environnement, validez les règles de sortie en vigueur à l’aide de l’API Cloud Manager. [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) opération.

   __requête HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Les configurations d’adresses IP sortantes dédiées peuvent être mises à jour à l’aide de l’API Cloud Manager. [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) opération. Mémoriser `enableEnvironmentAdvancedNetworkingConfiguration` est un `PUT` toutes les règles doivent donc être fournies avec chaque appel de cette opération.

1. Obtenez la variable __adresse IP de sortie dédiée__ en utilisant un résolveur DNS (tel que [DNSCchaker.org](https://dnschecker.org/)) sur l’hôte : `p{programId}.external.adobeaemcloud.com`ou en exécutant `dig` de la ligne de commande.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Notez que le nom d’hôte ne peut pas être `pinged`, car il s’agit d’une sortie et _not_ et d’entrée.

1. Vous pouvez désormais utiliser l’adresse IP de sortie dédiée dans votre code et configuration d’AEM personnalisé. Souvent, lors de l’utilisation d’une adresse IP de sortie dédiée, les services externes auxquels AEM as a Cloud Service se connecte sont configurés pour autoriser uniquement le trafic à partir de cette adresse IP dédiée.

## Connexion à des services externes via une sortie de port dédiée

Lorsque l’adresse IP de sortie dédiée est activée, le code AEM et la configuration peuvent utiliser l’adresse IP de sortie dédiée pour effectuer des appels vers des services externes. Il existe deux types d’appels externes qui AEM traitent différemment :

1. Appels HTTP/HTTPS aux services externes sur les ports non standard
   + Inclut les appels HTTP/HTTPS effectués aux services s’exécutant sur des ports autres que les ports 80 ou 443 standard.
1. Appels non HTTP/HTTPS aux services externes
   + Inclut tous les appels non HTTP, tels que les connexions aux serveurs de messagerie, aux bases de données SQL ou aux services qui s’exécutent sur d’autres protocoles non HTTP/HTTPS.

Les requêtes HTTP/HTTPS provenant d’AEM sur les ports standard (80/443) sont autorisées par défaut et ne nécessitent aucune configuration ni considérations supplémentaires.

>[!TIP]
>
> Consultez la documentation sur les adresses IP sortantes d’AEM as a Cloud Service pour [l’ensemble complet des règles de routage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing).


### HTTP/HTTPS sur les ports non standard

Lors de la création de connexions HTTP/HTTPS à des ports non standard (non 80/443) à partir d’AEM, la connexion doit être établie par le biais d’hôtes et de ports spéciaux, fournis via des espaces réservés.

AEM fournit deux ensembles de variables système Java™ spéciales qui correspondent à des proxys HTTP/HTTPS AEM.

| Nom de variable | Utilisation | Code Java™ | Configuration OSGi | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Hôte proxy pour les connexions HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | Port proxy pour les connexions HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` | Hôte proxy pour les connexions HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | Port proxy pour les connexions HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

Les demandes pour les services externes HTTP/HTTPS doivent être effectuées en configurant la configuration du proxy du client HTTP Java™ à l’aide des valeurs d’hôte/port du proxy AEM.

Lors d’appels HTTP/HTTPS à des services externes sur des ports non standard, aucun appel correspondant `portForwards` doit être défini à l’aide de l’API Cloud Manager `enableEnvironmentAdvancedNetworkingConfiguration` , car les &quot;règles&quot; de transfert de port sont définies &quot;dans le code&quot;.

#### Exemples de code

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS sur les ports non standard" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS sur les ports non standard</a></strong></div>
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

| Nom de variable | Utilisation | Code Java™ | Configuration OSGi | | - | - | - | - | | `AEM_PROXY_HOST` | Hôte proxy pour les connexions non HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Les connexions à des services externes sont ensuite appelées par l’intermédiaire de la fonction `AEM_PROXY_HOST` et le port mappé (`portForwards.portOrig`), qui AEM ensuite achemine vers le nom d’hôte externe mappé (`portForwards.name`) et port (`portForwards.portDest`).

| Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemples de code

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connexion SQL à l’aide de JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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