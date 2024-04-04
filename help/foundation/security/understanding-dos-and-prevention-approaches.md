---
title: Comprendre la prévention des attaques par déni de service (DoS)/démon de service (DDoS)
description: Découvrez comment prévenir et atténuer les attaques DoS et DDoS contre AEM.
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: f84a6cc54838e5bf87cfa173ef17df4ec745ebcb
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---

# Présentation de la prévention des attaques par déni de service (DoS) en AEM

Découvrez les options disponibles pour prévenir et atténuer les attaques DoS et DDoS sur votre environnement AEM. Avant de passer aux mécanismes de prévention, un bref aperçu de la [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) et [Début](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- Les attaques par déni de service (DoS) et par déni de service (DDoS) sont deux tentatives malveillantes visant à perturber le fonctionnement normal d’un serveur, d’un service ou d’un réseau ciblé, le rendant inaccessible aux utilisateurs prévus.
- Les attaques par déni de service proviennent généralement d’une seule source, tandis que les attaques par déni de service proviennent de sources multiples.
- Les attaques DDoS sont souvent plus volumineuses que les attaques DoS en raison des ressources combinées de plusieurs dispositifs d’attaque.
- Ces attaques sont menées en inondant la cible de trafic excessif et en exploitant les vulnérabilités des protocoles réseau.

Le tableau suivant décrit la manière de prévenir et d’atténuer les attaques par déni de service (DoS) et par déni de service (DDoS) :

<table>
    <tbody>
        <tr>
            <td><strong>Mécanisme de prévention</strong></td>
            <td><strong>Description</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (On-prem)</strong></td>
        </tr>
        <tr>
            <td>Web Application Firewall (WAF)</td>
            <td>Solution de sécurité conçue pour protéger les applications web de divers types d’attaques.</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">Licence de protection WAF-DDoS</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> ou <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF via le contrat AMS.</td>
            <td>Votre méthode WAF préférée</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (ou module Apache mod_security) est une solution Open Source et multi-plateformes qui offre une protection contre toute une gamme d’attaques contre les applications web.<br/> Dans AEM as a Cloud Service, cela s’applique uniquement au service de publication AEM, car il n’existe pas de serveur web Apache et de Dispatcher AEM devant le service de création.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Activation de ModSecurity </a></td>
        </tr>
        <tr>
            <td>Règles de filtrage du trafic</td>
            <td>Les règles de filtrage du trafic peuvent être utilisées pour bloquer ou autoriser les requêtes au niveau de la couche CDN.</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Exemples de règles de filtrage de trafic</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> ou <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> fonctions de limitation des règles.</td>
            <td>Votre solution préférée</td>
        </tr>
    </tbody>
</table>

## Analyse post-incident et amélioration continue

Il n’existe pas de flux standard unique pour l’identification et la prévention des attaques par déni de service (DoS/DDoS), mais cela dépend du processus de sécurité de votre entreprise. La variable **analyse post-incident et amélioration continue** est une étape cruciale du processus. Voici quelques bonnes pratiques à prendre en compte :

- Identifiez la cause première de l’attaque DoS/DDoS en effectuant une analyse post-incident, y compris en examinant les journaux, le trafic réseau et les configurations système.
- Améliorer les mécanismes de prévention à partir des résultats de l&#39;analyse post-incident.

