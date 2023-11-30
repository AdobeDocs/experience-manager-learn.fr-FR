---
title: Mise en réseau avancée
description: Découvrez les options de mise en réseau avancée d’AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 100%

---

# Mise en réseau avancée

AEM as a Cloud Service fournit des fonctionnalités de mise en réseau avancées qui permettent une gestion précise des connexions vers et depuis les programmes AEM as a Cloud Service.

|                                                   | [Programmes de production](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=fr) | [Programmes Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=fr) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Prise en charge de la mise en réseau avancée | ✔ | ✘ |


La mise en réseau avancée d’AEM comprend trois options pour gérer la connectivité avec des services externes. Un programme Cloud Manager et ses environnements AEM as a Cloud Service ne peuvent utiliser qu’un seul type de configuration réseau avancée à la fois, afin de vous assurer que le type le plus approprié est sélectionné.

|                                   | HTTP/HTTPS sur les ports standard | HTTP/HTTPS sur les ports non standard | Connexions non HTTP/HTTPS | Adresse IP de sortie dédiée | Liste des « Hôtes sans proxy » | Se connecter à des services protégés par VPN | Limiter le trafic de l’instance de publication AEM par IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Aucune mise en réseau avancée__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Sortie de port flexible__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Adresse IP de sortie dédiée__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Réseau privé virtuel (VPN)__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Pour plus d’informations sur les points à prendre en compte lors de la sélection du type de mise en réseau avancée approprié, veuillez consulter la [documentation relative à la mise en réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=fr).

## Tutoriels relatifs à la mise en réseau avancée

Une fois que l’option de mise en réseau avancée la plus appropriée en fonction des besoins de votre entreprise a été identifiée, cliquez sur le tutoriel correspondant ci-dessous pour obtenir des instructions détaillées et des exemples de code.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Sortie de port flexible" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Sortie de port flexible</a></strong></div>
      <p>
          Autorisez le trafic sortant d’AEM as a Cloud Service sur les ports non standard.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Adresse IP de sortie dédiée" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Adresse IP de sortie dédiée</a></strong></div>
      <p>
        Originez le trafic sortant d’AEM as a Cloud Service depuis une adresse IP dédiée.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Réseau privé virtuel (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Réseau privé virtuel (VPN)</a></strong></div>
      <p>
        Sécurisez le trafic entre une infrastructure cliente ou fournisseur et AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Exemples de code

Cette collection fournit des exemples de la configuration et du code requis pour tirer parti des fonctionnalités de mise en réseau avancée pour des cas d’utilisation spécifiques.

Assurez-vous que la [configuration de mise en réseau avancée](#advanced-networking) a été configurée avant de suivre ces tutoriels.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Réseau privé virtuel (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Service e-mail</a></strong></div>
      <p>
        Exemple de configuration OSGi utilisant AEM pour se connecter à des services de messagerie externes.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Exemple de code Java™ permettant d’établir une connexion HTTP/HTTPS depuis AEM as a Cloud Service vers un service externe à l’aide du protocole HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connexion SQL à l’aide de JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connexion SQL à l’aide de JDBC DataSourcePool</a></strong></div>
      <p>
            Exemple de code Java™ se connectant à des bases de données SQL externes en configurant le pool de la source de données JDBC d’AEM.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Connexion SQL à l’aide des API Java" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Connexion SQL à l’aide des API Java™</a></strong></div>
      <p>
            Exemple de code Java™ se connectant à des bases de données SQL externes à l’aide des API SQL de Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=fr"><img alt="Appliquer une liste d’adresses IP autorisées" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=fr">Appliquer une liste d’adresses IP autorisées</a></strong></div>
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
</tr>
</table>
