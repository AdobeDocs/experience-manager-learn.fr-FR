---
title: Adresse IP de sortie dédiée
description: Découvrez comment configurer et utiliser une adresse IP de sortie dédiée, qui permet aux connexions sortantes d’AEM d’être issues d’une adresse IP dédiée.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '1218'
ht-degree: 100%

---

# Adresse IP de sortie dédiée

Découvrez comment configurer et utiliser une adresse IP de sortie dédiée, qui permet aux connexions sortantes d’AEM d’être issues d’une adresse IP dédiée.

## Qu’est-ce que l’adresse IP de sortie dédiée ?

Une adresse IP de sortie dédiée permet aux requêtes d’AEM as a Cloud Service d’utiliser une adresse IP dédiée, ce qui permet aux services externes de filtrer les requêtes entrantes par cette adresse IP. Comme les [ports de sortie flexibles](./flexible-port-egress.md), une adresse IP de sortie dédiée vous permet de passer sur des ports non standards.

Un programme Cloud Manager ne peut avoir qu’un __seul__ type d’infrastructure réseau. Assurez-vous que l’adresse IP de sortie dédiée est le [type le plus approprié d’infrastructure réseau](./advanced-networking.md) pour votre instance AEM as a Cloud Service avant d’exécuter les commandes suivantes.

>[!MORELIKETHIS]
>
> Lisez la [documentation sur la configuration réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#dedicated-egress-IP-address) d’AEM as a Cloud Service pour plus d’informations sur les adresses IP de sorties dédiées.

## Conditions préalables

Les éléments suivants sont requis lors de la configuration d’une adresse IP de sortie dédiée :

+ API Cloud Manager avec [autorisations de la personne propriétaire d’entreprise Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Accès aux [informations d’identification de l’authentification à l’API Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + ID d’organisation (ou ID d’organisation IMS)
   + ID client (ou clé API)
   + Jeton d’accès (ou jeton porteur)
+ ID de programme Cloud Manager
+ ID d’environnement de Cloud Manager

Pour plus d’informations, regardez la présentation suivante pour découvrir comment installer, configurer et obtenir les informations d’identification de l’API Cloud Manager, et comment les utiliser pour effectuer un appel d’API Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Ce tutoriel utilise `curl` pour créer les configurations de l’API Cloud Manager. Les commandes `curl` fournies reposent sur la syntaxe Linux/macOS. Si vous utilisez l’invite de commande Windows, remplacez le caractère de saut de ligne `\` par `^`.

## Activer l’adresse IP de sortie dédiée sur le programme

Commencez par activer et configurer l’adresse IP de sortie dédiée sur AEM as a Cloud Service.

1. Tout d’abord, déterminez la région dans laquelle la mise en réseau avancée est nécessaire à l’aide de l’opération [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. Le `region name` est nécessaire pour effectuer les appels d’API Cloud Manager ultérieurs. En règle générale, la région dans laquelle l’environnement de production réside est utilisée.

   Recherchez la région de votre environnement AEM as a Cloud Service AEM dans [Cloud Manager](https://my.cloudmanager.adobe.com) sous les [détails de l’environnement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=fr#viewing-environment). Le nom de région affiché dans Cloud Manager peut être [mappé au code de région](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) utilisé dans l’API Cloud Manager.

   __Requête HTTP listRegion__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Activez l’adresse IP de sortie dédiée pour un programme Cloud Manager à l’aide de l’opération [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. Utilisez le code `region` approprié obtenu de l’opération `listRegions` de l’API Cloud Manager.

   __Requête HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Patientez 15 minutes pendant que le programme Cloud Manager approvisionne l’infrastructure réseau.

1. Vérifiez que le programme a terminé la configuration de l’__adresse IP de sortie dédiée__ à l’aide de l’opération [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) de l’API Cloud Manager et de l’`id` renvoyé par la requête HTTP createNetworkInfrastructure dans l’étape précédente.

   __Requête HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Vérifiez que la réponse HTTP contient un __statut__ __prêt__. Si elle n’est pas encore prête, revérifiez le statut quelques minutes plus tard.

## Configurer des proxys d’adresses IP de sorties dédiées par environnement

1. Définissez la configuration de l’__adresse IP de sortie dédiée__ sur chaque environnement AEM as a Cloud Service à l’aide de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager.

   __Requête HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Définissez les paramètres JSON dans un `dedicated-egress-ip-address.json` et fourni à curl via `... -d @./dedicated-egress-ip-address.json`.

   [Téléchargez l’exemple de dedicated-egress-ip-address.json](./assets/dedicated-egress-ip-address.json). Ce fichier n’est qu’un exemple. Configurez votre fichier selon les besoins en fonction des champs facultatifs/obligatoires documentés dans [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   La signature HTTP de la configuration de l’adresse IP de sortie dédiée diffère du [port de sortie flexible](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) uniquement parce qu’il prend également en charge la configuration facultative `nonProxyHosts`.

   `nonProxyHosts` déclare un ensemble d’hôtes pour lesquels le port 80 ou 443 doit être acheminé par les plages d’adresses IP partagées par défaut plutôt que par l’adresse IP de sortie dédiée. `nonProxyHosts` peut s’avérer utile, car le trafic sortant par les adresses IP partagées peut être optimisé automatiquement par Adobe.

   Pour chaque mappage `portForwards`, la mise en réseau avancée définit la règle de transfert suivante :

   | Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Pour chaque environnement, validez les règles de sortie en vigueur à l’aide de l’opération [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager.

   __Requête HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Les configurations d’adresses IP de sorties dédiées peuvent être mises à jour à l’aide de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. N’oubliez pas que `enableEnvironmentAdvancedNetworkingConfiguration` est une opération `PUT`, et que toutes les règles doivent par conséquent être fournies à chaque appel de cette opération.

1. Obtenez l’__adresse IP de sortie dédiée__ en utilisant un résolveur DNS (tel que [DNSChecker.org](https://dnschecker.org/)) sur l’hôte : `p{programId}.external.adobeaemcloud.com`, ou en exécutant `dig` à partir de la ligne de commande.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Le nom d’hôte ne peut pas être `pinged`, car il s’agit d’une sortie et _non_ d’une entrée.

   Notez que l’__adresse IP de sortie dédiée__ est partagé par tous les environnements AEM as a Cloud Service du programme.

1. Vous pouvez désormais utiliser l’adresse IP de sortie dédiée dans votre code personnalisé et dans votre configuration AEM. Souvent, lors de l’utilisation d’une adresse IP de sortie dédiée, les services externes auxquels AEM as a Cloud Service se connecte sont configurés pour autoriser uniquement le trafic à partir de cette adresse IP dédiée.

## Se connecter à des services externes sur une adresse IP de sortie dédiée

Lorsque l’adresse IP de sortie dédiée est activée, le code et la configuration AEM peuvent utiliser l’adresse IP de sortie dédiée pour effectuer des appels vers des services externes. Il existe deux types d’appels externes qu’AEM traite différemment :

1. Appels HTTP/HTTPS vers des services externes
   + Cela inclut les appels HTTP/HTTPS effectués aux services s’exécutant sur des ports autres que les ports 80 ou 443 standard.
1. Appels non HTTP/HTTPS aux services externes
   + Cela inclut tous les appels non HTTP, tels que les connexions aux serveurs de messagerie, aux bases de données SQL ou aux services qui s’exécutent sur d’autres protocoles non HTTP/HTTPS.

Les requêtes HTTP/HTTPS provenant d’AEM sur les ports standard (80/443) sont autorisées par défaut, mais elles n’utilisent pas l’adresse IP de sortie dédiée si elles ne sont pas correctement configurées comme décrit ci-dessous.

>[!TIP]
>
> Consultez la documentation sur les adresses IP de sorties dédiées d’AEM as a Cloud Service pour l’[ensemble complet des règles de routage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Lors de la création de connexions HTTP/HTTPS à partir d’AEM et de l’utilisation d’adresses IP de sortie dédiées, les connexions HTTP/HTTPS sont automatiquement traitées par proxy hors d’AEM à l’aide de l’adresse IP de sortie dédiée. La prise en charge des connexions HTTP/HTTPS ne nécessite pas de code ou de configuration supplémentaire.

#### Exemples de code

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Exemple de code Java™ permettant d’établir une connexion HTTP/HTTPS depuis AEM as a Cloud Service vers un service externe à l’aide du protocole HTTP/HTTPS.
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
| `AEM_PROXY_HOST` | Hôte proxy pour les connexions non HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Les connexions à des services externes sont ensuite appelées par l’intermédiaire de `AEM_PROXY_HOST` et du port mappé (`portForwards.portOrig`), qu’AEM achemine ensuite vers le nom d’hôte externe mappé (`portForwards.name`) et le port (`portForwards.portDest`).

| Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Exemples de code

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connexion SQL à l’aide de JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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
