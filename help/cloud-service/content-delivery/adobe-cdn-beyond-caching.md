---
title: Adobe CDN - Fonctionnalités avancées au-delà de la mise en cache
description: Découvrez les fonctionnalités avancées du réseau de diffusion de contenu Adobe au-delà de la mise en cache, comme la configuration du trafic sur le réseau de diffusion de contenu, les pages d’erreur sur le réseau de diffusion de contenu, etc.
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
source-git-commit: cc7fa9bbaa775b0216c839be079504c2b5467878
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---


# Adobe CDN - Fonctionnalités avancées au-delà de la mise en cache

Découvrez les fonctionnalités avancées du réseau de diffusion de contenu Adobe au-delà de la mise en cache, comme la configuration du trafic sur le réseau de diffusion de contenu, les pages d’erreur sur le réseau de diffusion de contenu, etc.

Outre la mise en cache de contenu, Adobe CDN propose plusieurs fonctionnalités avancées qui peuvent vous aider à optimiser les performances de votre site web. Ces fonctionnalités sont les suivantes :

- Configuration du trafic sur le réseau de diffusion de contenu
- Configuration des informations d’identification et de l’authentification du réseau de diffusion de contenu
- Pages d’erreur CDN

Ces fonctionnalités sont des **fonctions en libre-service**. Configuré dans le fichier `cdn.yaml` de votre projet AEM et déployé à l’aide du pipeline de configuration Cloud Manager.

## Configuration du trafic sur le réseau de diffusion de contenu

Découvrez les principales fonctionnalités liées à _Configuration du trafic sur le réseau de diffusion de contenu_ :

- **Prévention des attaques par déni de service :** l’Adobe CDN absorbe les attaques par déni de service sur le réseau
, les empêchant d’atteindre votre serveur d’origine.
- **Limitation de débit :** Pour éviter que votre serveur d’origine ne soit submergé par trop de requêtes, vous pouvez configurer la limitation de débit sur le réseau de diffusion de contenu.
- **Pare-feu d’applications web (WAF) :** WAF protège votre site web contre les vulnérabilités d’applications web courantes, telles que l’injection SQL, les scripts intersites, etc. La licence de sécurité améliorée ou la licence de protection WAF-DDoS est requise pour utiliser cette fonctionnalité.
- **Transformation de requêtes :** modifiez les requêtes entrantes telles que la définition ou la non-définition d’en-têtes, la modification des paramètres de requête, des cookies, etc.
- **Transformation de réponse :** modifiez les réponses sortantes telles que la définition ou la non-définition d’en-têtes.
- **Choix de l’origine :** acheminez le trafic vers différents serveurs d’origine (Adobe et non-Adobe) en fonction de l’URL de demande.
- **Redirection d’URL :** demandes de redirection (HTTP 301/302) vers une URL absolue ou relative différente.

## Configuration des informations d’identification et de l’authentification du réseau de diffusion de contenu

Découvrez les principales fonctionnalités liées à la _configuration des informations d’identification et de l’authentification CDN_ :

- **Purge API Token** : permet de créer votre propre clé de purge pour la purge d’un seul groupe ou de toutes les ressources du cache.
- **Authentification de base** : mécanisme d’authentification léger lorsque vous souhaitez restreindre l’accès à votre site web ou à une partie de celui-ci. Cela est principalement nécessaire dans le cadre de divers processus de révision avant la mise en ligne.
- **Validation d’en-tête HTTP** : utilisé lorsqu’un réseau de diffusion de contenu géré par le client achemine le trafic vers le réseau de diffusion de contenu Adobe. Le CDN Adobe valide la requête entrante en fonction de la valeur d’en-tête `X-AEM-Edge-Key`.
Permet de créer votre propre valeur pour l’en-tête `X-AEM-Edge-Key`.

## Pages d’erreur CDN

Comprenons les principales fonctionnalités liées aux _pages d’erreur CDN_ :

- **Pages d’erreur de marque** : affichez une page d’erreur de marque à vos utilisateurs dans le _scénario improbable_ lorsque le réseau de diffusion de contenu Adobe ne parvient pas à atteindre votre serveur d’origine.

## Comment mettre en oeuvre

La mise en oeuvre de ces fonctionnalités avancées implique deux étapes :

1. **Mettre à jour le fichier de configuration CDN** : mettez à jour le fichier `cdn.yaml` de votre projet AEM avec les configurations requises. Les configurations sont ajoutées en tant que règles et suivent une syntaxe de règle. La règle trois composants principaux : `name`, `when` et `action`.

2. **Déployer le fichier de configuration CDN** : déployez le fichier `cdn.yaml` mis à jour à l’aide du pipeline de configuration Cloud Manager. Pour plus d’informations, voir [Déploiement de règles via Cloud Manager](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

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

## Ressources supplémentaires

[Protection des sites web avec des règles de filtrage du trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[Configuration et déploiement de la règle CDN de validation d’en-tête HTTP](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Comment purger le cache CDN](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Configuration du trafic sur le CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Configuration des informations d’identification et de l’authentification CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

[Configuration des pages d’erreur CDN](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-error-pages)




