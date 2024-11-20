---
title: Réseau CDN Adobe - Fonctionnalités avancées au-delà de la mise en cache
description: Découvrez les fonctionnalités avancées du réseau CDN Adobe au-delà de la mise en cache, comme la configuration du trafic sur le réseau CDN, la configuration des jetons et des informations d’identification, les pages d’erreur du réseau CDN, etc.
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 8795024a7b5e6d10cb2ff2f770dd3d080af85e68
workflow-type: ht
source-wordcount: '546'
ht-degree: 100%

---

# Réseau CDN Adobe - Fonctionnalités avancées au-delà de la mise en cache

Découvrez les fonctionnalités avancées du réseau de diffusion de contenu (CDN) Adobe au-delà de la mise en cache, telles que la configuration du trafic sur le réseau CDN, la configuration des jetons et des informations d’identification, les pages d’erreur du réseau CDN, etc.

Outre la mise en cache de contenu, le réseau CDN Adobe propose plusieurs fonctionnalités avancées qui peuvent vous aider à optimiser les performances de votre site web. Ces fonctionnalités incluent les suivantes :

- Configuration du trafic sur le réseau CDN
- Configuration des informations d’identification et de l’authentification du réseau CDN
- Pages d’erreur du réseau CDN

Ces fonctionnalités sont des fonctionnalités en **libre-service**. Configuré dans le fichier `cdn.yaml` de votre projet AEM et déployé à l’aide du pipeline de configuration Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## Configuration du trafic sur le réseau CDN

Découvrez les principales fonctionnalités liées à la _Configuration du trafic sur le réseau CDN_ :

- **Prévention des attaques par déni de service (DoS) :** le réseau CDN Adobe absorbe les attaques par déni de service sur la couche réseau, ce qui les empêche d’atteindre votre serveur d’origine.
- **Limitation de débit :** pour éviter que votre serveur d’origine ne soit submergé par trop de requêtes, vous pouvez configurer la limitation de débit sur le réseau CDN.
- **Pare-feu d’applications web (WAF) :** le pare-feu d’application web protège votre site web contre les vulnérabilités d’applications web courantes, telles que l’injection SQL, le cross-site scripting, etc. La licence de sécurité améliorée ou la licence de protection WAF-DDoS est requise pour utiliser cette fonctionnalité.
- **Transformation de requêtes :** modifiez les requêtes entrantes telles que la définition ou l’annulation de la définition d’en-têtes, la modification des paramètres de requête, les cookies, etc.
- **Transformation de réponse :** modifiez les réponses sortantes telles que la définition ou l’annulation de la définition d’en-têtes.
- **Sélection de l’origine :** acheminez le trafic vers différents serveurs d’origine (Adobe et non-Adobe) en fonction de l’URL de la demande.
- **Redirection d’URL :** demandes de redirection (HTTP 301/302) vers une URL absolue ou relative différente.

## Configuration des informations d’identification et de l’authentification du réseau CDN

Découvrez les principales fonctionnalités liées à la _configuration des informations d’identification et de l’authentification de réseau CDN_ :

- **Jeton Purger l’API** : permet de créer votre propre clé de purge pour la purge d’une seule ressource, d’un groupe de ressources ou de toutes les ressources du cache.
- **Authentification de base** : mécanisme d’authentification léger lorsque vous souhaitez restreindre l’accès à votre site web ou à une partie de celui-ci. Cela est principalement nécessaire dans le cadre de divers processus de révision avant la mise en ligne.
- **Validation d’en-tête HTTP** : utilisé lorsqu’un réseau CDN géré par le client ou la cliente achemine le trafic vers le réseau CDN Adobe. Le réseau CDN Adobe valide la requête entrante en fonction de la valeur d’en-tête `X-AEM-Edge-Key`. Permet de créer votre propre valeur pour l’en-tête `X-AEM-Edge-Key`.

## Pages d’erreur du réseau CDN

Passons en revue les principales fonctionnalités liées aux _pages d’erreur de réseau CDN_ :

- **Pages d’erreur de marque** : affichez une page d’erreur de marque à vos utilisateurs et utilisatrices dans le _scénario improbable_ lorsque le réseau CDN Adobe ne parvient pas à atteindre votre serveur d’origine.

## Implémentation

L’implémentation de ces fonctionnalités avancées implique deux étapes :

1. **Mettre à jour le fichier de configuration CDN** : mettez à jour le fichier `cdn.yaml` de votre projet AEM avec les configurations requises. Les configurations sont ajoutées en tant que règles et suivent une syntaxe de règle. La règle est composée de trois composants principaux : `name`, `when` et `action`.

2. **Déployer le fichier de configuration CDN** : déployez le fichier `cdn.yaml` mis à jour à l’aide du pipeline de configuration Cloud Manager. Pour plus d’informations, voir [Déploiement de règles via Cloud Manager](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Exemple

Dans l’exemple ci-dessous, l’exemple de site WKND est configuré pour rediriger l’URL `/top3` vers `/us/en/top3.html`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Tutoriels associés

[Protection des sites web avec des règles de filtrage du trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[Configurer et déployer la règle de réseau CDN de validation d’en-tête HTTP](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Purge du cache du réseau CDN](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Configuration des pages d’erreur du réseau CDN](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[Configuration du trafic sur le réseau CDN](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Configuration des informations d’identification et de l’authentification du réseau CDN](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

