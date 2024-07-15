---
title: Outil d’analyse des journaux CDN
description: Découvrez l’outil d’analyse des journaux AEM Cloud Service CDN fourni par Adobe et la manière dont il permet d’obtenir des informations à la fois sur les performances du réseau de diffusion de contenu et sur la mise en oeuvre AEM.
version: Cloud Service
feature: Developer Tools
topic: Development
role: Developer, Architect, Admin
level: Beginner
doc-type: Tutorial
duration: 219
last-substantial-update: 2024-05-17T00:00:00Z
jira: KT-15505
thumbnail: KT-15505.jpeg
exl-id: 830c2486-099b-454f-bc07-6bf36e81ac8d
source-git-commit: 4111ae0cf8777ce21c224991b8b1c66fb01041b3
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 1%

---

# Outil d’analyse des journaux CDN

Découvrez l’ _outil d’analyse des journaux AEM Cloud Service CDN_ fourni par Adobe et la manière dont il permet d’obtenir des informations sur les performances de votre réseau de diffusion de contenu et l’implémentation d’AEM.
 
>[!VIDEO](https://video.tv.adobe.com/v/3429177?quality=12&learn=on)

## Vue d’ensemble

L’ [ outil d’analyse des journaux AEM as a Cloud Service CDN](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling) offre des tableaux de bord prédéfinis que vous pouvez intégrer à la [pile Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) ou à la [pile ELK](https://www.elastic.co/elastic-stack) pour la surveillance et l’analyse en temps réel de vos journaux CDN.

Grâce à cet outil, vous pouvez effectuer une surveillance en temps réel et une détection proactive des problèmes. Il s’agit donc d’assurer une diffusion de contenu optimisée et des mesures de sécurité appropriées contre les attaques par déni de service (DoS) et par déni de service (DDoS).

## Fonctions clés

- Analyse de journal simplifiée
- Surveillance en temps réel
- Intégration transparente
- Tableaux de bord pour
   - Identifier les menaces potentielles de sécurité
   - Expérience utilisateur final plus rapide

## Présentation du tableau de bord

Pour lancer rapidement l’analyse des journaux, Adobe fournit des tableaux de bord prédéfinis pour la pile Splunk et ELK.

- **Taux d’accès au cache de réseau CDN** : fournit des informations sur le taux d’accès au cache total et le nombre total de requêtes par état HIT, PASS et MISS. Il fournit également les principales URL d’accès, de PASS et de MISS.

  ![Taux d’accès au cache CDN](assets/CHR-dashboard.png)

- **Tableau de bord du trafic CDN** : fournit des insights sur le trafic via les taux de demande CDN et d’origine, les taux d’erreur 4xx et 5xx, et les demandes non mises en cache. Il fournit également un nombre maximal de demandes de CND et d’origine par seconde par adresse IP du client, ainsi que des informations supplémentaires pour optimiser les configurations de CDN.

  ![Tableau de bord du trafic CDN](assets/Traffic-dashboard.png)

- **Tableau de bord WAF** : fournit des informations par le biais de requêtes analysées, marquées et bloquées. Il fournit également les principales attaques par l’identifiant d’indicateur de WAF, les 100 premiers attaquants par adresse IP du client, pays et agent utilisateur, ainsi que des informations supplémentaires pour optimiser les configurations de WAF.

  ![Tableau de bord WAF](assets/WAF-Dashboard.png)

## Intégration Splunk

Pour les organisations qui utilisent [Splunk](https://www.splunk.com/en_us/products/observability-cloud.html) et qui ont activé le transfert de journal AEMCS vers leurs instances Splunk, peut rapidement importer des tableaux de bord préconfigurés. Cette configuration facilite l’analyse accélérée des journaux, fournissant ainsi des informations exploitables pour optimiser les mises en oeuvre d’AEM et atténuer les menaces de sécurité telles que les attaques DOS.

Vous pouvez commencer à utiliser le guide [Splunk dashboard for AEMCS CDN Log Analysis](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/Splunk/README.md#splunk-dashboards-for-aemcs-cdn-log-analysis).


## Intégration ELK

La [pile ELK](https://www.elastic.co/elastic-stack), composée d’Elasticsearch, de Logstash et de Kibana, est une autre option puissante pour l’analyse des logs. Elle est utile pour les organisations qui n’ont pas accès à une configuration Splunk ou à des fonctionnalités de transfert de journal. La configuration locale de la pile ELK est simple, l’outil fournit le fichier Docker Composer pour démarrer rapidement. Vous pouvez ensuite importer les tableaux de bord prédéfinis et ingérer les journaux CDN téléchargés à l’aide d’Adobe Cloud Manager.

Vous pouvez commencer à utiliser le guide [ELK Docker container for AEMCS CDN Log Analysis](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling/blob/main/ELK/README.md#elk-docker-container-for-aemcs-cdn-log-analysis) .
