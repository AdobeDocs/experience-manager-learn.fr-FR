---
title: Réseau privé virtuel (VPN)
description: Découvrez comment connecter AEM as a Cloud Service à votre VPN pour créer des canaux de communication sécurisés entre AEM et les services internes.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: bccaccd386d065720cddfd689cbadc220609b8a8
workflow-type: tm+mt
source-wordcount: '1319'
ht-degree: 99%

---

# Réseau privé virtuel (VPN)

Découvrez comment connecter AEM as a Cloud Service à votre VPN pour créer des canaux de communication sécurisés entre AEM et les services internes.

## Qu’est-ce que le réseau privé virtuel ?

Le réseau privé virtuel (VPN) permet à un client ou une cliente AEM as a Cloud Service de connecter **les environnements AEM** d’un programme Cloud Manager à un VPN existant et [pris en charge](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#vpn). Cela permet des connexions sécurisées et contrôlées entre AEM as a Cloud Service et les services au sein du réseau du client ou de la cliente.

Un programme Cloud Manager ne peut avoir qu’un __seul__ type d’infrastructure réseau. Assurez-vous que le réseau privé virtuel est le type le plus [approprié d’infrastructure réseau](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#general-vpn-considerations) pour votre instance AEM as a Cloud Service avant d’exécuter les commandes suivantes.

>[!NOTE]
>
>Veuillez noter que la connexion de l’environnement de création de Cloud Manager à un VPN n’est pas prise en charge. Si vous devez accéder aux artefacts binaires d’un référentiel privé, vous devez configurer un référentiel sécurisé et protégé par mot de passe avec une URL disponible sur l’Internet public [tel que décrit ici](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html?lang=fr#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> Veuillez consulter la [documentation sur la configuration réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#vpn) pour AEM as a Cloud Service pour plus d’informations sur le réseau privé virtuel.

## Conditions préalables

Les éléments suivants sont requis lors de la configuration du réseau privé virtuel :

+ Compte Adobe avec [autorisations de la personne propriétaire d’entreprise Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/).
+ Accès aux [informations d’authentification de l’API Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/).
   + ID d’organisation (ou ID d’organisation IMS)
   + ID client (ou clé API)
   + Jeton d’accès (ou jeton porteur)
+ ID de programme Cloud Manager
+ ID d’environnement de Cloud Manager
+ A **Basé sur des itinéraires** Réseau privé virtuel, avec accès à tous les paramètres de connexion nécessaires.

Pour plus d’informations, regardez la présentation suivante pour découvrir comment installer, configurer et obtenir les informations d’identification de l’API Cloud Manager, et comment les utiliser pour effectuer un appel d’API Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

Ce tutoriel utilise `curl` pour créer les configurations de l’API Cloud Manager. Les commandes `curl` fournies reposent sur la syntaxe Linux/macOS. Si vous utilisez l’invite de commande Windows, remplacez le caractère de saut de ligne `\` par `^`.

## Activer le réseau privé virtuel par programme

Commencez par activer le réseau privé virtuel sur AEM as a Cloud Service.

1. Tout d’abord, déterminez la région dans laquelle le réseau avancé est nécessaire à l’aide de l’opération [listRegion](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. Le `region name` est nécessaire pour effectuer les appels d’API Cloud Manager ultérieurs. En règle générale, la région dans laquelle l’environnement de production réside est utilisée.

   Recherchez la région de votre environnement AEM as a Cloud Service AEM dans [Cloud Manager](https://my.cloudmanager.adobe.com) sous les [détails de l’environnement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=fr#viewing-environment). Le nom de région affiché dans Cloud Manager peut être [mappé au code de région](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) utilisé dans l’API Cloud Manager.

   __Requête HTTP listRegions__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Activez le réseau privé virtuel pour un programme Cloud Manager à l’aide de l’opération [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. Utilisez le code `region` approprié obtenu de l’opération `listRegions` de l’API Cloud Manager.

   __Requête HTTP createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Définissez les paramètres JSON d’un objet `vpn-create.json` fourni à cURL via l’objet `... -d @./vpn-create.json`.

   [Téléchargez l’exemple vpn-create.json](./assets/vpn-create.json).  Ce fichier n’est qu’un exemple. Configurez votre fichier selon les besoins en fonction des champs facultatifs/obligatoires documentés dans [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
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

   Patientez 45-60 minutes pendant que le programme Cloud Manager approvisionne l’infrastructure réseau.

1. Vérifiez que l’environnement a terminé la configuration du __réseau privé virtuel__ à l’aide de l’opération [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) de l’API Cloud Manager, en utilisant l’`id` renvoyé par la requête HTTP createNetworkInfrastructure de l’étape précédente.

   __Requête HTTP getNetworkInfrastructure__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Vérifiez que la réponse HTTP renvoie le __statut__ __prêt__. Le cas échéant, vérifiez le statut après quelques minutes.

## Configurer les proxys de réseau privé virtuel par environnement

1. Activez et configurez la configuration du __réseau privé virtuel__ sur chaque environnement AEM as a Cloud Service à l’aide de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager.

   __Requête HTTP enableEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Définissez les paramètres JSON d’un objet `vpn-configure.json` fourni à cURL via l’objet `... -d @./vpn-configure.json`.

[Télécharger l’exemple vpn-configure.json](./assets/vpn-configure.json)

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

   `nonProxyHosts` déclare un ensemble d’hôtes pour lesquels le port 80 ou 443 doit être acheminé par les plages d’adresses IP partagées par défaut plutôt que par l’adresse IP de sortie dédiée. `nonProxyHosts` peut s’avérer utile, car le trafic sortant par les adresses IP partagées peut être optimisé automatiquement par Adobe.

   Pour chaque mappage `portForwards`, la mise en réseau avancée définit la règle de transfert suivante :

   | Hôte du proxy | Port du proxy |  | Hôte externe | Port externe |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Si votre déploiement AEM nécessite __uniquement__ des connexions HTTP/HTTPS vers un service externe, laissez le tableau `portForwards` vide, car ces règles ne s’appliquent qu’aux requêtes autres que HTTP/HTTPS.


1. Pour chaque environnement, validez les règles de routage VPN en vigueur à l’aide de l’opération [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager.

   __Requête HTTP getEnvironmentAdvancedNetworkingConfiguration__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Les configurations de réseau privé virtuel peuvent être mises à jour à l’aide de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) de l’API Cloud Manager. N’oubliez pas que `enableEnvironmentAdvancedNetworkingConfiguration` est une opération `PUT`, et que toutes les règles doivent par conséquent être fournies à chaque appel de cette opération.

1. Vous pouvez désormais utiliser la configuration de réseau privé virtuel dans votre code AEM personnalisé et dans votre configuration.

## Se connecter à des services externes via un réseau privé virtuel

Lorsque le réseau privé virtuel est activé, le code et la configuration AEM peuvent les utiliser pour effectuer des appels vers des services externes via le VPN. Il existe deux types d’appels externes qu’AEM traite différemment :

1. Appels HTTP/HTTPS vers des services externes
   + Cela inclut les appels HTTP/HTTPS effectués aux services s’exécutant sur des ports autres que les ports 80 ou 443 standard.
1. Appels non HTTP/HTTPS aux services externes
   + Cela inclut tous les appels non HTTP, tels que les connexions aux serveurs de messagerie, aux bases de données SQL ou aux services qui s’exécutent sur d’autres protocoles non HTTP/HTTPS.

Les requêtes HTTP/HTTPS provenant d’AEM sur les ports standard (80/443) sont autorisées par défaut, mais elles n’utilisent pas la connexion au VPN si elles ne sont pas correctement configurées comme décrit ci-dessous.

### HTTP/HTTPS

Lors de la création de connexions HTTP/HTTPS à partir d’AEM, lors de l’utilisation de VPN, les connexions HTTP/HTTPS sont automatiquement traitées par proxy hors d’AEM. La prise en charge des connexions HTTP/HTTPS ne nécessite pas de code ou de configuration supplémentaire.

>[!TIP]
>
> Consultez la documentation sur le réseau privé virtuel d’AEM as a Cloud Service pour connaître [l’ensemble des règles de routage](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#vpn-traffic-routing).

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

### Exemples de code de connexions non HTTP/HTTPS

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

### Limiter l’accès à AEM as a Cloud Service via VPN

La configuration du réseau privé virtuel limite l’accès aux environnements AEM as a Cloud Service à un VPN.

#### Exemples de configurations

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=fr"><img alt="Appliquer une liste d’adresses IP autorisées" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=fr">Appliquer une liste d’adresses IP autorisées</a></strong></div>
      <p>
            Configurez une liste d’adresses IP autorisées de sorte que seul le trafic VPN puisse accéder à AEM.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#restrict-vpn-to-ingress-connections"><img alt="Restrictions d’accès VPN basées sur un chemin d’accès au service de publication AEM" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr#restrict-vpn-to-ingress-connections">Restrictions d’accès VPN basées sur un chemin d’accès au service de publication AEM</a></strong></div>
      <p>
            Exigez un accès VPN pour des chemins d’accès spécifiques au service de publication AEM.
      </p>
    </td>
   <td></td>
</tr></table>
