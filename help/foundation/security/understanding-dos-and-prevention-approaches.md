---
title: Comprendre la prévention des attaques par déni de service (DoS)/déni de service distribué (DDoS)
description: Découvrez comment prévenir et atténuer les attaques DoS et DDoS contre AEM.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 75
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 100%

---

# Comprendre la prévention des attaques par déni de service (DoS)/déni de service distribué (DDoS) dans AEM

Découvrez les options disponibles pour prévenir et atténuer les attaques DoS et DDoS sur votre environnement AEM. Avant de passer aux mécanismes de prévention, une brève vue d’ensemble de [DoS](https://developer.mozilla.org/fr-FR/docs/Glossary/DOS_attack) et de [DDoS](https://developer.mozilla.org/fr-FR/docs/Glossary/Distributed_Denial_of_Service).

- Les attaques par déni de service (DoS) et par déni de service distribué (DDoS) sont deux tentatives malveillantes visant à perturber le fonctionnement normal d’un serveur, d’un service ou d’un réseau ciblé, le rendant inaccessible aux utilisateurs et utilisatrices prévus.
- Les attaques DoS proviennent généralement d’une seule source, tandis que les attaques DDoS proviennent de sources multiples.
- Les attaques DDoS sont souvent plus volumineuses que les attaques DoS en raison des ressources combinées de plusieurs dispositifs d’attaque.
- Ces attaques sont menées en inondant la cible de trafic excessif et en exploitant les vulnérabilités des protocoles réseau.

Le tableau suivant décrit comment prévenir et atténuer les attaques DoS et DDoS :

<table>
    <tbody>
        <tr>
            <td><strong>Mécanisme de prévention</strong></td>
            <td><strong>Description</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (On-Prem)</strong></td>
        </tr>
        <tr>
            <td>Pare-feu d’application web (WAF)</td>
            <td>Solution de sécurité conçue pour protéger les applications web de divers types d’attaques.</td>
            <td>
            <a href="https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">Licence de protection WAF-DDoS</a></td>
            <td><a href="https://docs.aws.amazon.com/fr_fr/waf/" target="_blank">AWS</a> ou <a href="https://azure.microsoft.com/fr-fr/products/web-application-firewall" target="_blank">Azure</a> WAF via le contrat AMS.</td>
            <td>Votre WAF préféré</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (ou module Apache « mod_security ») est une solution open source sur plusieurs plateformes qui offre une protection contre un éventail d’attaques à l’encontre des applications web.<br/> Dans AEM as a Cloud Service, cela s’applique uniquement au service de publication AEM, car il n’existe pas de serveur web Apache et de Dispatcher AEM devant le service de création.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/fr/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Activer ModSecurity </a></td>
        </tr>
        <tr>
            <td>Règles de filtrage du trafic</td>
            <td>Les règles de filtrage du trafic peuvent être utilisées pour bloquer ou autoriser les requêtes au niveau de la couche du réseau CDN.</td>
            <td><a href="https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Exemple de règles de filtrage du trafic</a></td>
            <td>Fonctions de limitation de règles <a href="https://docs.aws.amazon.com/fr_fr/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> ou <a href="https://learn.microsoft.com/fr-fr/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>.</td>
            <td>Votre solution préférée</td>
        </tr>
    </tbody>
</table>

## Analyse post-incident et amélioration continue

Il n’existe pas de flux standard unique pour l’identification et la prévention des attaques DoS/DDoS, et cela dépend du processus de sécurité de votre entreprise. L’**analyse post-incident et l’amélioration continue** constituent une étape cruciale du processus. Voici quelques bonnes pratiques à prendre en compte :

- Identifiez la cause première de l’attaque DoS/DDoS en effectuant une analyse post-incident, y compris en examinant les journaux, le trafic réseau et les configurations système.
- Améliorez les mécanismes de prévention à partir des résultats de l’analyse post-incident.

