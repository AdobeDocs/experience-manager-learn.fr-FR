---
title: Mise en réseau avancée
description: Découvrez les options de mise en réseau avancées d’AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 8%

---

# Mise en réseau avancée

AEM as a Cloud Service fournit des fonctionnalités de mise en réseau avancées qui permettent une gestion précise des connexions à et depuis AEM programmes as a Cloud Service.

|  | [Programmes de production](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Programmes Sandbox](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Prise en charge de la mise en réseau avancée | ✔ | ✘ |


AEM mise en réseau avancée comprend trois options pour gérer la connectivité avec des services externes. Un programme Cloud Manager et ses environnements as a Cloud Service AEM ne peuvent utiliser qu’un seul type de configuration réseau avancée à la fois, afin de vous assurer que le type le plus approprié est sélectionné.

|  | HTTP/HTTPS sur les ports standard | HTTP/HTTPS sur les ports non standard | Connexions non HTTP/HTTPS | IP sortante dédiée | Liste &quot;Hôtes sans proxy&quot; | Connexion à des services protégés par VPN | Limiter le trafic de publication AEM par IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Pas de mise en réseau avancée__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Sortie de port flexible__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Adresse IP sortante dédiée__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Réseau privé virtuel__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Pour plus d’informations sur les points à prendre en compte lors de la sélection du type de mise en réseau avancé approprié, voir [documentation de mise en réseau avancée](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutoriels réseau avancés

Une fois que l’option de mise en réseau avancée la plus appropriée en fonction des besoins de votre entreprise a été identifiée, cliquez dans le tutoriel correspondant ci-dessous pour obtenir des instructions étape par étape et des exemples de code.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Sortie de port flexible" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Sortie de port flexible</a></strong></div>
      <p>
          Autoriser le trafic as a Cloud Service AEM sortant sur les ports non standard.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Adresse IP sortante dédiée à FileDeress" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Adresse IP sortante dédiée</a></strong></div>
      <p>
        Originez AEM trafic as a Cloud Service sortant à partir d’une adresse IP dédiée.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Réseau privé virtuel (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Réseau privé virtuel (VPN)</a></strong></div>
      <p>
        Trafic sécurisé entre une infrastructure client ou fournisseur et AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Exemples de code

Cette collection fournit des exemples de la configuration et du code requis pour tirer parti des fonctionnalités de mise en réseau avancées pour des cas d’utilisation spécifiques.

Assurez-vous que la variable [configuration réseau avancée](#advanced-networking) a été configuré avant de suivre ces tutoriels.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Réseau privé virtuel (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">Service de messagerie électronique</a></strong></div>
      <p>
        Exemple de configuration OSGi utilisant AEM pour se connecter à des services de messagerie externes.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Exemple de code Java™ permettant d’établir une connexion HTTP/HTTPS d’AEM as a Cloud Service à un service externe à l’aide du protocole HTTP/HTTPS.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="Connexion SQL à l’aide de JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">Connexion SQL à l’aide de JDBC DataSourcePool</a></strong></div>
      <p>
            Exemple de code Java™ se connectant à des bases de données SQL externes en configurant AEM pool de sources de données JDBC.
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
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=fr"><img alt="Application d’une liste autorisée IP" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Application d’une liste autorisée IP</a></strong></div>
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
</tr>
</table>
