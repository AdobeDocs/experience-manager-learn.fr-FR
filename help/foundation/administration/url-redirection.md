---
title: Redirections d’URL
description: Découvrez les différentes options permettant d'effectuer la redirection URL dans AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: 00ea3a8e6b69cd99cf293093d38b59df51f6a26d
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 3%

---


# Redirections d’URL

La redirection d’URL est un aspect courant dans le cadre du fonctionnement du site web. Les architectes et les administrateurs doivent trouver la meilleure solution pour gérer les redirections d’URL, pour plus de flexibilité et un temps de déploiement de redirection rapide.

Assurez-vous que vous connaissez les [AEM (6.x) ou AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) et [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) infrastructure. Les principales différences sont les suivantes :

1. AEM as a Cloud Service a [réseau de diffusion de contenu intégré](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr)Toutefois, les clients peuvent fournir un réseau de diffusion de contenu (BYOCDN) devant le réseau de diffusion de contenu géré par AEM.
1. AEM 6.x, qu’on-premise ou Adobe Managed Services (AMS) n’inclut pas de réseau de diffusion de contenu AEM géré, et les clients doivent apporter les leurs.

Les autres services d’AEM (AEM Author/Publish et Dispatcher) sont similaires sur le plan conceptuel entre AEM 6.x et les services as a Cloud Service.

AEM les solutions de redirection d’URL sont les suivantes :

|  | Géré et déployé en tant que code de projet AEM | Possibilité de modifier par l’équipe marketing/contenu | AEM compatible avec Cloud Service | Où l&#39;exécution de la redirection se produit |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Sur Edge via votre propre réseau de diffusion de contenu](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` règles en tant que configuration de Dispatcher ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - Gestionnaire des cartes de redirection](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - Gestionnaire de redirection](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [Le `Redirect` propriété de page](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Options de solution

Vous trouverez ci-dessous des options de solution afin de vous rapprocher du navigateur du visiteur du site web.

### Sur Edge via votre propre réseau de diffusion de contenu

Certains services CDN proposent des solutions de redirection au niveau de Edge, réduisant ainsi les allers-retours vers l’origine. Voir [Redirecteur Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [Fonctions AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Veuillez consulter votre fournisseur de service CDN pour connaître la fonctionnalité de redirection au niveau Edge.

La gestion des redirections au niveau du réseau Edge ou CDN présente des avantages en termes de performances. Toutefois, elles ne sont pas gérées dans le cadre de projets AEM mais plutôt de projets discrets. Pour éviter des problèmes, un processus bien pensé pour gérer et déployer les règles de redirection est essentiel.


### Apache `mod_rewrite` module

Une solution commune utilise [Module Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). Le [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype) fournit une structure de projet Dispatcher pour les deux [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) et [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) projet. Les règles de réécriture personnalisées et par défaut (non modifiables) sont définies dans la variable `conf.d/rewrites` et que le moteur de réécriture est activé pour `virtualhosts` qui écoute sur le port `80` via `conf.d/dispatcher_vhost.conf` fichier . Un exemple de mise en oeuvre est disponible dans la section [AEM projet WKND Sites](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

Dans AEM as a Cloud Service, ces règles de redirection sont gérées dans le cadre du code AEM et déployées via Cloud Manager. [Pipeline de configuration de niveau web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) ou [Pipeline à pile complète](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). Ainsi, votre processus spécifique au projet AEM est en jeu pour gérer, déployer et suivre les règles de redirection.

La plupart des services CDN mettent en cache les redirections HTTP 301 et 302 en fonction de leurs `Cache-Control` ou `Expires` en-têtes. Cela permet d’éviter l’aller-retour après la redirection initiale en provenance d’Apache/Dispatcher.


### ACS AEM Commons

Deux fonctionnalités sont disponibles dans [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) pour gérer les redirections d’URL. Veuillez noter qu&#39;ACS AEM Commons est un projet communautaire et open source qui n&#39;est pas pris en charge par Adobe.

#### Gestionnaire des cartes de redirection

[Gestionnaire des cartes de redirection](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) permet aux administrateurs d’AEM 6.x de facilement gérer et publier [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) sans accéder directement au serveur web Apache ni nécessiter un redémarrage du serveur web Apache. Cette fonctionnalité permet aux utilisateurs des autorisations de créer, mettre à jour et supprimer des règles de redirection à partir d’une console dans AEM, sans l’aide de l’équipe de développement ou d’un déploiement AEM. Le Gestionnaire de mappage de redirection **NON AEM compatible as a Cloud Service**.

#### Gestionnaire de redirection

[Gestionnaire de redirection](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permet aux utilisateurs d’AEM de gérer et de publier facilement les redirections à partir d’AEM. L’implémentation est basée sur le filtre de servlet Java™, ce qui permet une consommation de ressources JVM standard. Cette fonctionnalité élimine également la dépendance à l’égard de l’équipe de développement AEM et des déploiements AEM. Le Gestionnaire de redirection est **AEM as a Cloud Service** et **AEM 6.x** compatible. Bien que la requête de redirection initiale doive accéder au service de publication AEM pour générer le 301/302 de cache de 301/302 (la plupart) CDN par défaut, ce qui permet aux requêtes suivantes d’être redirigées vers le serveur Edge/CDN.

### Le `Redirect` propriété de page

La clé en main `Redirect` de la propriété de page [Onglet Avancé](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) permet aux auteurs de contenu de définir l’emplacement de redirection de la page active. Cette solution est préférable pour les scénarios de redirection par page et ne dispose pas d’un emplacement central pour afficher et gérer les redirections de page.

## Quelle solution convient à la mise en oeuvre ?

Vous trouverez ci-dessous quelques critères pour déterminer la bonne solution. Le processus informatique et marketing de votre entreprise doit également vous aider à choisir la bonne solution.

1. Permet à l’équipe marketing ou aux super-utilisateurs de gérer les règles de redirection sans l’équipe de développement AEM et les déploiements AEM.
1. Processus de gestion, de vérification, de suivi et d’annulation des modifications ou de l’atténuation des risques.
1. Disponibilité de _Expertise en matière de sujet_ pour **À Edge via le service CDN** solution.

