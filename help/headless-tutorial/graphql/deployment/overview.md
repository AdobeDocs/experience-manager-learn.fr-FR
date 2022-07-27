---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# AEM Déploiements sans affichage

AEM les déploiements de clients sans affichage prennent de nombreuses formes ; SPA hébergé par AEM, site web ou web externe, application mobile ou même processus serveur à serveur.

En fonction du client et de la manière dont il est déployé, AEM déploiements sans affichage ont des considérations différentes.

## Architecture AEM service

Avant d&#39;explorer les considérations liées au déploiement, il est impératif de comprendre AEM architecture logique, ainsi que la séparation et les rôles des niveaux de service d&#39;AEM as a Cloud Service. AEM as a Cloud Service comprend deux services logiques :

+ __Auteur AEM__ est le service où les équipes créent, collaborent et publient des fragments de contenu (et d’autres ressources).
+ __Publication AEM__ est le service vers lequel les fragments de contenu publiés (et d’autres ressources) sont répliqués pour une utilisation générale.

AEM clients sans affichage opérant dans une capacité de production interagissent généralement avec AEM Publish, qui contient le contenu approuvé et publié. Le client qui interagit avec l’auteur AEM doit prendre en compte de manière particulière, car l’auteur AEM est sécurisé par défaut, ce qui nécessite une autorisation pour toutes les requêtes, ainsi que du contenu incomplet ou non approuvé.

## Déploiements de clients sans affichage

+ [Application d’une seule page](./spa.md)
+ [Composant Web/JS](./web-component.md)
+ [Application mobile](./mobile.md)
+ [Serveur vers serveur](./server-to-server.md)

