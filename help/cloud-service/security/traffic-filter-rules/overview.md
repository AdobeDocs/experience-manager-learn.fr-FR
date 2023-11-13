---
title: Protéger les sites web avec des règles de filtrage du trafic (y compris des règles WAF)
description: Découvrez les règles de filtrage du trafic, y compris la sous-catégorie de règles WAF (pare-feu d’application web). Découvrez comment créer, déployer et tester les règles. Analysez également les résultats et protégez vos sites AEM.
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
ht-degree: 100%

---

# Protéger les sites web avec des règles de filtrage du trafic (y compris des règles WAF)

En savoir plus sur les **règles de filtrage de trafic**, y compris la sous-catégorie de **Règles WAF (pare-feu d’application web)** dans AEM as a Cloud Service (AEMCS). Commencez votre lecture et apprenez à créer, déployer et tester les règles. Analysez également les résultats et protégez vos sites AEM.

>[!VIDEO](https://video.tv.adobe.com/v/3425401?quality=12&learn=on)

## Vue d’ensemble

La réduction des risques liés aux violations de la sécurité est une priorité absolue pour toute organisation. À cette fin, AEM dispose de la fonctionnalité de règles de filtrage du trafic, y compris les règles WAF, pour protéger les sites web et les applications.

Les règles de filtrage du trafic sont déployés sur le [réseau CDN intégré](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr) et sont évaluées avant que la requête n’atteigne l’infrastructure AEM. Grâce à cette fonctionnalité, vous pouvez améliorer considérablement la sécurité de votre site web, en vous assurant que seules les demandes légitimes sont autorisées à accéder à l’infrastructure AEM.

Ce tutoriel vous guide tout au long du processus de création, de déploiement, de test et d’analyse des résultats des règles de filtrage du trafic, y compris des règles WAF.

Pour en savoir plus sur les règles de filtrage du trafic, consultez [cet article](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=fr).

>[!IMPORTANT]
>
> Les « règles WAF » constituent une sous-catégorie des règles de filtrage du trafic. Elles nécessitent une licence de protection WAF-DDoS ou de sécurité améliorée.

Nous vous invitons à faire part de vos commentaires ou à poser des questions sur les règles de filtrage de trafic en envoyant un e-mail à l’adresse suivante : **aemcs-waf-adopter@adobe.com**.

## Étape suivante

Commencez à [configurer](./how-to-setup.md) cette fonctionnalité pour créer, déployer et tester des règles de filtrage du trafic. En savoir plus sur la configuration des outils de tableau de bord de la pile **Elasticsearch, Logstash et Kibana (ELK)** pour analyser les résultats des journaux du réseau CDN AEMCS.


