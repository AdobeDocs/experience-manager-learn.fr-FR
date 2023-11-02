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
exl-id: e6d67204-2f76-441c-a178-a34798fe266d
source-git-commit: ceb498f751ffc50d0022a16b63f9f52594bc507e
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 3%

---

# Protection des sites web avec des règles de filtrage du trafic (y compris les règles WAF)

En savoir plus **règles de filtrage de trafic**, y compris sa sous-catégorie de **Règles WAF (Web Application Firewall)** dans AEM as a Cloud Service (AEMCS). Découvrez comment créer, déployer et tester les règles. Analysez également les résultats pour protéger vos sites AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Vue d’ensemble

La réduction des risques d’infraction à la sécurité est une priorité absolue pour toute organisation. AEM propose la fonctionnalité de règles de filtrage du trafic, y compris les règles WAF, pour protéger les sites web et les applications.

Les règles de filtrage du trafic sont déployées sur la variable [réseau de diffusion de contenu intégré](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr) et sont évalués avant que la requête n’atteigne l’infrastructure AEM. Grâce à cette fonctionnalité, vous pouvez améliorer considérablement la sécurité de votre site web, en vous assurant que seules les demandes légitimes sont autorisées à accéder à l’infrastructure AEM.

Ce tutoriel vous guide tout au long du processus de création, de déploiement, de test et d’analyse des résultats des règles de filtrage du trafic, y compris les règles WAF.

Vous pouvez en savoir plus sur les règles de filtrage du trafic dans [cet article](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en).

>[!IMPORTANT]
>
> Une sous-catégorie de règles de filtrage du trafic appelée &quot;règles WAF&quot; nécessite une licence de protection WAF-DDoS ou une licence de sécurité améliorée.

Nous vous invitons à faire part de vos commentaires ou à poser des questions sur les règles de filtrage de trafic en envoyant un courrier électronique. **aemcs-waf-adopter@adobe.com**.

## Étape suivante

Formation [configuration](./how-to-setup.md) Cette fonctionnalité vous permet de créer, déployer et tester des règles de filtrage du trafic. En savoir plus sur la configuration du **Elasticsearch, Logstash et Kibana (ELK)** l’outil stack dashboard pour analyser les résultats de vos journaux CDN AEMCS.


