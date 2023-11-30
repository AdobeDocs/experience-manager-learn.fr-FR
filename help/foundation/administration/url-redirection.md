---
title: Redirections d’URL
description: Découvrez les différentes options dédiées à la redirection des URL dans AEM.
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 100%

---

# Redirections d’URL

La redirection d’URL est une opération standard de toute conception de site web. Les personnes chargées de l’architecture et de l’administration doivent rivaliser d’ingéniosité dans la gestion des redirections d’URL, afin d’offrir plus de flexibilité et un délai de redirection plus rapide.

Assurez-vous que vous connaissez les bases de l’infrastructure d’[AEM (6.x) ou AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html?lang=fr#the-%E2%80%9Clegacy%E2%80%9D-setup) et d’[AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html?lang=fr#runtime-architecture). Les principales différences sont les suivantes :

1. AEM as a Cloud Service dispose d’un [réseau CDN intégré](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr). Toutefois, la clientèle peut activer un réseau CDN (BYOCDN) autre que le réseau CDN géré par AEM.
1. AEM 6.x, On-Premise ou Adobe Managed Services (AMS), n’inclut pas de réseau CDN géré par AEM, la clientèle doit fournir son propre réseau CDN.

Il en va de même pour les autres services AEM (création et publication, et Dispatcher) et les différences entre AEM 6.x et AEM as a Cloud Service sont également de rigueur.

Les solutions de redirection d’URL proposées par AEM sont les suivantes :

|                                                   | Géré et déployé en tant que code de projet AEM | Possibilité de modification par l’équipe de marketing/contenu | Compatible avec AEM as a Cloud Service | Emplacement de l’exécution de la redirection |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [Sur Edge via votre propre réseau CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/réseau CDN |
| [Apache, règles `mod_rewrite` en tant que configuration de Dispatcher](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons : gestionnaire de mappage de redirection](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons : gestionnaire de redirection](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [Propriété de page `Redirect`](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## Solutions

Les solutions suivantes sont présentées dans l’ordre où elles sont les plus proches du navigateur du visiteur ou de la visiteuse du site web.

### Sur Edge via votre propre réseau CDN

Certains services CDN proposent des solutions de redirection au niveau d’Edge, réduisant ainsi les allers-retours vers l’origine. Consultez les articles [Redirection Akamai Edge](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector) et [Fonctions AWS CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Contactez votre fournisseur de service CDN à propos de la fonctionnalité de redirection au niveau Edge.

La gestion des redirections au niveau du réseau Edge ou CDN présente des avantages en termes de performances. Toutefois, elles ne sont pas gérées dans le cadre de projets AEM mais plutôt de projets distincts. Pour éviter des problèmes, un processus réfléchi pour gérer et déployer les règles de redirection est essentiel.


### Module Apache `mod_rewrite`

Une solution courante consiste à utiliser le [Module Apache mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). L’[Archétype de projet AEM](https://github.com/adobe/aem-project-archetype) fournit une structure de projet Dispatcher pour les projets [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) et [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure). Les règles de réécriture personnalisées et par défaut (non modifiables) sont définies dans le dossier `conf.d/rewrites` et le moteur de réécriture est activé pour `virtualhosts`, qui écoute sur le port `80` via le fichier `conf.d/dispatcher_vhost.conf`. Un exemple de mise en œuvre est disponible dans la section [Projet AEM WKND Sites](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

Dans AEM as a Cloud Service, ces règles de redirection sont gérées dans le cadre du code AEM et déployées via le [Pipeline de configuration de niveau web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr#web-tier-config-pipelines) ou le [Pipeline full-stack](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr#full-stack-pipeline) de Cloud Manager. Ainsi, le processus spécifique à votre projet AEM intervient dans la gestion, le déploiement et la traçabilité des règles de redirection.

La plupart des services de réseau CDN mettent en cache les redirections HTTP 301 et 302 en fonction de leurs en-têtes `Cache-Control` ou `Expires`. Cela permet d’éviter l’aller-retour après la redirection initiale en provenance d’Apache/du Dispatcher.


### ACS AEM Commons

Deux fonctionnalités d’[ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) sont dédiées à la gestion des redirections d’URL. Veuillez noter qu’ACS AEM Commons est un projet communautaire et open source qui n’est pas pris en charge par Adobe.

#### Gestionnaire de mappage de redirection

Le [Gestionnaire de mappage de redirection](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) permet aux administrateurs et aux administratrices d’AEM 6 x de gérer et de publier les fichiers [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) en toute facilité, sans besoin d’accès direct ou de redémarrage du serveur web Apache. Cette fonctionnalité permet aux utilisateurs et aux utilisatrices dotés des autorisations appropriées de créer, mettre à jour et supprimer des règles de redirection à partir d’une console dans AEM, sans l’aide de l’équipe de développement ou d’un déploiement AEM. Le Gestionnaire de mappage de redirection **n’est PAS compatible avec AEM as a Cloud Service**.

#### Gestionnaire de redirection

Les [Gestionnaire de redirection](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) permet aux utilisateurs et aux utilisatrices d’AEM de gérer et de publier en toute facilité les redirections à partir d’AEM. L’implémentation est basée sur le filtre de servlet Java™, ce qui permet une consommation de ressources JVM standard. Cette fonctionnalité élimine également la dépendance à l’égard de l’équipe de développement AEM et des déploiements AEM. Le Gestionnaire de redirection est compatible avec **AEM as a Cloud Service** et **AEM 6.x**. Bien que la requête de redirection initiale doive accéder au service de publication AEM pour générer les redirections HTTP 301/302, la plupart des réseaux CDN mettent en cache ces redirections HTTP 301/302 par défaut, ce qui permet aux requêtes suivantes d’être redirigées vers le réseau Edge/CDN.

### La propriété de page `Redirect`

La propriété de page prête à l’emploi `Redirect` de l’[Onglet Avancé](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html?lang=fr#advanced) permet aux auteurs et autrices de contenu de définir l’emplacement de redirection de la page active. Cette solution est préférable pour les scénarios de redirection par page et ne dispose pas d’un emplacement central pour afficher et gérer les redirections de page.

## Quelle solution se prête le mieux à la mise en œuvre ?

Vous trouverez ci-dessous quelques critères pour déterminer la bonne solution. Le processus informatique et marketing de votre entreprise doit également apporter son grain de sel et vous aider à choisir la bonne solution.

1. Permettre à l’équipe marketing ou aux super-utilisateurs et super-utilisatrices de gérer les règles de redirection sans l’équipe de développement AEM ni les déploiements AEM.
1. Processus de gestion, de vérification, de suivi et d’annulation des modifications ou de l’atténuation des risques.
1. Disponibilité d’une _Expertise en la matière_ pour la solution **Sur Edge via le service de réseau CDN**.
