---
title: Réseau privé virtuel (VPN)
description: Découvrez comment connecter AEM as a Cloud Service à votre VPN pour créer des canaux de communication sécurisés entre AEM et les services internes.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '1261'
ht-degree: 1%

---


# Réseau privé virtuel (VPN)

Découvrez comment connecter AEM as a Cloud Service à votre VPN pour créer des canaux de communication sécurisés entre AEM et les services internes.

## Qu’est-ce que le réseau privé virtuel ?

Le réseau privé virtuel (VPN) permet à un client as a Cloud Service AEM de connecter un programme Cloud Manager à un programme existant, [pris en charge](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) VPN. Cela permet des connexions sécurisées et contrôlées entre AEM as a Cloud Service et les services au sein du réseau du client.

Un programme Cloud Manager ne peut avoir qu’une __single__ type d’infrastructure réseau. S’assurer que le réseau privé virtuel est le plus performant [type approprié d’infrastructure réseau](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html%3Flang%3Dja#general-vpn-considerations) pour votre AEM as a Cloud Service avant d’exécuter les commandes suivantes.

>[!MORELIKETHIS]
>
> Lisez l’AEM as a Cloud Service [documentation sur la configuration réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) pour plus d’informations sur le réseau privé virtuel.

## Prérequis

Les éléments suivants sont requis lors de la configuration du réseau privé virtuel :

+ Compte Adobe avec [Autorisations du propriétaire commercial Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Accès à [Informations d’identification d’authentification de l’API Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID d’organisation (alias ID d’organisation IMS)
   + ID client (alias clé API)
   + Jeton d’accès (ou jeton porteur)
+ ID de programme Cloud Manager
+ ID d’environnement de Cloud Manager
+ Un réseau privé virtuel, avec accès à tous les paramètres de connexion nécessaires.

Ce tutoriel utilise `curl` pour créer les configurations de l’API Cloud Manager. Le `curl` supposent une syntaxe Linux/macOS. Si vous utilisez l’invite de commande Windows, remplacez la variable `\` caractère de saut de ligne avec `^`.

## Activer le réseau privé virtuel par programme

Commencez par activer le réseau privé virtuel sur AEM as a Cloud Service.

1. Tout d’abord, déterminez la région dans laquelle la mise en réseau avancée sera configurée à l’aide de l’API Cloud Manager. [listRegion](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) opération. Le `region name` sera nécessaire pour effectuer les appels d’API Cloud Manager suivants. En règle générale, la région dans laquelle réside l’environnement de production est utilisée.

   __requête HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Activation du réseau privé virtuel pour un programme Cloud Manager à l’aide des API Cloud Manager [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) opération. Utilisez les `region` code obtenu à partir de l’API Cloud Manager `listRegions` opération.

   __requête HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Définition des paramètres JSON dans une `vpn-create.json` et fourni à curl via `... -d @./vpn-create.json`.

[Téléchargez l’exemple vpn-create.json](./assets/vpn-create.json)

   ```json
   { 
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [ 
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22", 
               "10.151.202.22",
               "10.154.155.22"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Patientez 45 à 60 minutes pendant que le programme Cloud Manager approvisionne l’infrastructure réseau.

1. Vérifier que l&#39;environnement est terminé __Réseau privé virtuel__ configuration à l’aide de l’API Cloud Manager [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) , à l’aide de la fonction `id` renvoyée par la requête HTTP createNetworkInfrastructure de l’étape précédente.

   __requête HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Vérifiez que la réponse HTTP contient une __status__ de __ready__. Si vous n’êtes pas encore prêt, vérifiez l’état toutes les quelques minutes.

## Configuration de proxys de réseau privé virtuel par environnement

1. Activez et configurez les __Réseau privé virtuel__ configuration sur chaque environnement as a Cloud Service AEM à l’aide de l’API Cloud Manager [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) opération.

   __requête HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Définition des paramètres JSON dans une `vpn-configure.json` et fourni à curl via `... -d @./vpn-configure.json`.

[Téléchargez l’exemple vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
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

   `nonProxyHosts` déclare un ensemble d’hôtes pour lequel le port 80 ou 443 doit être acheminé par les plages d’adresses IP partagées par défaut plutôt que par l’adresse IP sortante dédiée. Cela peut s’avérer utile, car le trafic passant par les adresses IP partagées peut être optimisé automatiquement par Adobe.

   Pour chaque `portForwards` , la mise en réseau avancée définit la règle de transfert suivante :

   | Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si votre déploiement AEM __only__ nécessite des connexions HTTP/HTTPS à un service externe ; laissez la variable `portForwards` vide, car ces règles ne sont requises que pour les requêtes non HTTP/HTTPS.


1. Pour chaque environnement, validez les règles de routage vpn en vigueur à l’aide de l’API Cloud Manager. [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) opération.

   __requête HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Les configurations de proxy de réseau privé virtuel peuvent être mises à jour à l’aide de l’API Cloud Manager. [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) opération. Mémoriser `enableEnvironmentAdvancedNetworkingConfiguration` est un `PUT` toutes les règles doivent donc être fournies avec chaque appel de cette opération.

1. Vous pouvez désormais utiliser la configuration de sortie de réseau privé virtuel dans votre code d’AEM personnalisé et dans votre configuration.

## Connexion à des services externes via le réseau privé virtuel

Lorsque le réseau privé virtuel est activé, le code AEM et la configuration peuvent les utiliser pour lancer des appels à des services externes via le VPN. Il existe deux types d’appels externes qui AEM traitent différemment :

1. Appels HTTP/HTTPS aux services externes sur les ports non standard
   + Inclut les appels HTTP/HTTPS effectués aux services s’exécutant sur des ports autres que les ports 80 ou 443 standard.
1. Appels non HTTP/HTTPS aux services externes
   + Inclut tous les appels non HTTP, tels que les connexions aux serveurs de messagerie, aux bases de données SQL ou aux services qui s’exécutent sur d’autres protocoles non HTTP/HTTPS.

Les requêtes HTTP/HTTPS provenant d’AEM sur les ports standard (80/443) sont autorisées par défaut et ne nécessitent aucune configuration ni considérations supplémentaires.

### HTTP/HTTPS sur les ports non standard

Lors de la création de connexions HTTP/HTTPS à des ports non standard (non 80/443) à partir d’AEM, la connexion doit être établie par le biais d’hôtes et de ports spéciaux, fournis via des espaces réservés.

AEM fournit deux ensembles de variables système Java™ spéciales qui correspondent à des proxys HTTP/HTTPS AEM.

| Nom de variable | Utilisation | Code Java™ | Configuration OSGi | Configuration mod_proxy du serveur web Apache | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Hôte proxy pour les connexions HTTP | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Port proxy pour les connexions HTTP | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Hôte proxy pour les connexions HTTPS | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Port proxy pour les connexions HTTPS | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

Les demandes pour les services externes HTTP/HTTPS doivent être effectuées en configurant la configuration du proxy du client HTTP Java™ via les valeurs d’hôte/port du proxy AEM.

Lors d’appels HTTP/HTTPS à des services externes sur des ports non standard, aucun appel correspondant `portForwards` doit être défini à l’aide des API de Cloud Manager `__enableEnvironmentAdvancedNetworkingConfiguration` , car les &quot;règles&quot; de transfert de port sont définies &quot;dans le code&quot;.

>[!TIP]
>
> Voir la documentation d’AEM sur le réseau privé virtuel d’as a Cloud Service pour [l’ensemble complet des règles de routage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn-traffic-routing).

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

### Exemples de code de connexions non HTTP/HTTPS

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

### Limiter l&#39;accès à AEM as a Cloud Service via VPN

La configuration du réseau privé virtuel permet de limiter l’accès aux environnements as a Cloud Service AEM à l’accès VPN.

#### Exemples de configuration

<table><tr>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=fr"><img alt="Application d’une liste autorisée IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en">Application d’une liste autorisée IP</a></strong></div>
      <p>
            Configurez une liste autorisée IP de sorte que seul le trafic VPN puisse accéder à AEM.
      </p>
    </td>   
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Restrictions d’accès VPN basées sur un chemin d’accès à AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Restrictions d’accès VPN basées sur un chemin d’accès à AEM Publish</a></strong></div>
      <p>
            Nécessite un accès VPN pour des chemins spécifiques sur AEM Publish.
      </p>
    </td>
   <td></td>   
</tr></table>
