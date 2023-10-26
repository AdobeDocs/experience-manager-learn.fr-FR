---
title: Protection des sites web avec des règles de filtrage du trafic (y compris les règles WAF)
description: Découvrez les règles de filtrage du trafic, y compris sa sous-catégorie de règles de pare-feu d’applications web (WAF). Comment créer, déployer et tester les règles. Analysez également les résultats pour protéger vos sites AEM.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 4%

---


# Protection des sites web avec des règles de filtrage du trafic (y compris les règles WAF)

En savoir plus **règles de filtrage de trafic**, y compris sa sous-catégorie de **Règles WAF (Web Application Firewall)** dans AEM as a Cloud Service (AEMCS). Découvrez comment créer, déployer et tester les règles. Analysez également les résultats pour protéger vos sites AEM.

## Vue d’ensemble

La réduction des risques d’infraction à la sécurité est une priorité absolue pour toute organisation. AEM propose la fonctionnalité de règles de filtrage du trafic, y compris les règles WAF, pour protéger les sites web et les applications.

Les règles de filtrage du trafic sont déployées sur la variable [réseau de diffusion de contenu intégré](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr) et sont évalués avant que la requête n’atteigne l’infrastructure AEM. Grâce à cette fonctionnalité, vous pouvez améliorer considérablement la sécurité de votre site web, en vous assurant que seules les demandes légitimes sont autorisées à accéder à l’infrastructure AEM.

Ce tutoriel vous guide tout au long du processus de création, de déploiement, de test et d’analyse des résultats des règles de filtrage du trafic, y compris les règles WAF.

Vous pouvez en savoir plus sur les règles de filtrage du trafic dans [cet article](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en)

>[!IMPORTANT]
>
> Une sous-catégorie de règles de filtrage du trafic appelée &quot;règles WAF&quot; nécessite une licence de protection WAF-DDoS .


## Étape suivante

Formation [configuration](./how-to-setup.md) Cette fonctionnalité vous permet de créer, déployer et tester des règles de filtrage du trafic. En savoir plus sur la configuration du **Elasticsearch, Logstash et Kibana (ELK)** l’outil stack dashboard pour analyser les résultats de vos journaux CDN AEMCS.



