---
title: Restriction de l’accès
description: Découvrez comment restreindre l’accès en bloquant des requêtes spécifiques à l’aide des règles de filtrage de trafic dans AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
exl-id: 53cb8996-4944-4137-a979-6cf86b088d42
source-git-commit: 71454ea9f1302d8d1c08c99e937afefeda2b1322
workflow-type: ht
source-wordcount: '390'
ht-degree: 100%

---

# Restriction de l’accès

Découvrez comment restreindre l’accès en bloquant des requêtes spécifiques à l’aide des règles de filtrage de trafic dans AEM as a Cloud Service.

Ce tutoriel explique comment **bloquer les requêtes vers des chemins d’accès internes à partir d’adresses IP publiques** dans le service de publication AEM.

## Pourquoi et comment bloquer des requêtes

Bloquer le trafic permet d’appliquer les politiques de sécurité de l’organisation en empêchant l’accès aux ressources ou URL sensibles sous certaines conditions. Par rapport à la journalisation, le blocage est une action plus stricte et doit être utilisé lorsqu’il est certain que le trafic provenant de sources spécifiques n’est pas autorisé ou est indésirable.

Les scénarios courants dans lesquels le blocage est approprié incluent :

- Limiter l’accès aux pages `internal` ou `confidential` aux plages d’adresses IP internes uniquement (par exemple, derrière un VPN d’entreprise).
- Bloquer le trafic de robots, de scanners automatisés ou d’acteurs de menaces identifiés par adresse IP ou géolocalisation.
- Empêcher l’accès à des points d’entrée obsolètes ou non sécurisés lors des migrations intermédiaires.
- Limiter l’accès aux outils de création ou aux itinéraires d’administration dans les niveaux de publication.

## Prérequis

Avant de poursuivre, assurez-vous d’avoir terminé la configuration requise, comme décrit dans le tutoriel [Configuration du filtre de trafic et des règles WAF](../setup.md). En outre, vous avez cloné et déployé le [projet WKND AEM Sites](https://github.com/adobe/aem-guides-wknd) dans votre environnement AEM.

## Exemple : blocage des chemins internes pour les adresses IP publiques

Dans cet exemple, vous configurez une règle pour bloquer l’accès externe à une page WKND interne, telle que `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`, pour les adresses IP publiques. Seuls les utilisateurs et utilisatrices appartenant à une plage d’adresses IP de confiance (comme un VPN d’entreprise) peuvent accéder à cette page.

Vous pouvez créer votre propre page interne (par exemple, `demo-page.html`) ou utiliser le [package joint](../assets/how-to/demo-internal-pages-package.zip).

- Ajoutez la règle suivante au fichier de projet WKND `/config/cdn.yaml`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- Validez et envoyez les modifications au référentiel Git de Cloud Manager.

- Déployez les modifications dans l’environnement AEM à l’aide du pipeline de configuration Cloud Manager [créé précédemment](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Testez la règle en accédant à la page interne du site WKND, par exemple `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` ou à l’aide de la commande CURL ci-dessous :

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Répétez l’étape ci-dessus à partir de l’adresse IP que vous avez utilisée dans la règle, puis d’une autre adresse IP (en utilisant votre téléphone mobile, par exemple).

## Analyser

Pour analyser les résultats de la règle `block-internal-paths`, suivez les étapes décrites dans le [tutoriel de configuration](../setup.md#cdn-logs-ingestion).

Vous devriez voir les **Requêtes bloquées** et les valeurs correspondantes dans les colonnes adresse IP du client (cli_ip), hôte, URL, action (waf_action) et nom de la règle (waf_match).

![Requête bloquée dans le tableau de bord de l’outil ELK](../assets/how-to/elk-tool-dashboard-blocked.png)
