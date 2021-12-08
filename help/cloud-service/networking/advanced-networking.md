---
title: Mise en réseau avancée
description: Découvrez les options de mise en réseau avancées d’AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# Mise en réseau avancée

AEM as a Cloud Service propose trois options pour gérer la connectivité avec les services externes. Un programme Cloud Manager et ses environnements as a Cloud Service AEM ne peuvent utiliser qu’un seul type de configuration réseau avancée à la fois, afin de vous assurer que le type le plus approprié est sélectionné.

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
