---
title: Options de noms de domaine personnalisés
description: Découvrez comment gérer et implémenter des noms de domaine personnalisés pour votre site web hébergé AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
exl-id: e11ff38c-e823-4631-a5b0-976c2d11353e
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 100%

---

# Options de noms de domaine personnalisés

Découvrez comment gérer et implémenter des noms de domaine pour votre site web hébergé AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## Avant de commencer

Avant de commencer à implémenter des noms de domaine personnalisés, veillez à comprendre les concepts suivants :

### Présentation d’un nom de domaine

Un nom de domaine est le nom convivial du site web, tel qu’adobe.com, qui pointe vers un emplacement spécifique (adresse IP du type 170.2.14.16) sur Internet.

### Noms de domaine par défaut dans AEM as a Cloud Service

Par défaut, AEM as a Cloud Service est configuré avec un nom de domaine par défaut, se terminant par `*.adobeaemcloud.com`. Le certificat SSL de caractères joker émis à l’encontre `*.adobeaemcloud.com` est automatiquement appliqué à tous les environnements et ce certificat de caractères joker est de la responsabilité d’Adobe.

Les noms de domaine par défaut sont au format `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com`.

- `<SERVICE-TYPE>` peut être **créer**, **publier** ou **prévisualiser**.
- `<PROGRAM-ID>` est l’identifiant unique du programme. Une organisation peut avoir plusieurs programmes.
- `<ENVIRONMENT-ID>` est l’identifiant unique de l’environnement et chaque programme contient quatre environnements : **Développement rapide (RDE)**, **dev**, **évaluation** et **prod**. Chaque environnement contient les trois types de service mentionnés ci-dessus, à l’exception de **RDE** qui n’a pas d’environnement de prévisualisation.

En résumé, une fois tous les environnements AEM as a Cloud Service configurés, vous disposez de **11** URL uniques (RDE n’a pas d’environnement de prévisualisation) combinées avec le nom de domaine par défaut.

### Réseau CDN géré par Adobe et réseau CDN géré par le client ou la cliente

Pour réduire la latence et améliorer les performances du site web, AEM as a Cloud Service est intégré à un réseau de diffusion de contenu (CDN) géré par Adobe. Le réseau CDN géré par Adobe est automatiquement activé pour tous les environnements. Pour plus d’informations, voir [Mise en cache d’AEM as a Cloud Service](../caching/overview.md).

Cependant, les clientes et clients peuvent également utiliser leur propre réseau CDN, appelé **réseau CDN géré par le client ou la cliente**. Cela n’est pas nécessaire, mais peu de personnes l’utilisent pour des politiques d’entreprise ou pour d’autres raisons. Dans ce cas, la personne est responsable de la gestion des configurations et des paramètres du réseau CDN.

### Noms de domaine personnalisés

Les noms de domaine personnalisés sont toujours préférés aux noms de domaine par défaut à des fins d’image de marque, d’authenticité et de développement commercial. Cependant, ils peuvent uniquement être appliqués aux types de services **publication** et **prévisualisation**, et non **création**.

Lors de l’ajout de noms de domaine personnalisés, vous devez fournir un certificat SSL valide pour le domaine personnalisé donné. Le certificat SSL doit être un certificat valide signé par une autorité de certification (CA) approuvée.

En règle générale, les clientes et les clients utilisent un nom de domaine personnalisé pour les environnements de production (site web AEM as a Cloud Service) et parfois pour les environnements inférieurs comme **évaluation** ou **développement**.

| Type de service AEM | Domaine personnalisé pris en charge ? |
|---------------------|:-----------------------:|
| Création | ✘ |
| Prévisualisation | ✔ |
| Publication | ✔ |

## Implémentation des noms de domaine

Pour implémenter des noms de domaine à l’aide d’un réseau CDN géré par Adobe ou d’un réseau CDN géré par le client ou la cliente, l’organigramme suivant vous guide tout au long du processus :

![Organigramme de gestion des noms de domaine](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

Le tableau suivant vous indique également où gérer les configurations spécifiques :

| Nom de domaine personnalisé avec | Ajouter un certificat SSL à | Ajouter le nom de domaine à | Configurer les enregistrements DNS à l’adresse | Vous avez besoin d’une règle de réseau CDN de validation d’en-tête HTTP ? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| Réseau CDN géré par Adobe | Adobe Cloud Manager | Adobe Cloud Manager | Service d’hébergement DNS | ✘ |
| Réseau CDN géré par le client ou la cliente | Fournisseur de réseau CDN | Fournisseur de réseau CDN | Service d’hébergement DNS | ✔ |

### Tutoriels détaillés

Maintenant que vous comprenez le processus de gestion des noms de domaine, vous pouvez implémenter des noms de domaine personnalisés pour votre site web AEM as a Cloud Service en suivant les tutoriels ci-dessous :

**[Noms de domaine personnalisés avec réseau CDN géré par Adobe](./custom-domain-name-with-adobe-managed-cdn.md)** : dans ce tutoriel, vous apprenez à ajouter un nom de domaine personnalisé à un **site web AEM as a Cloud Service avec réseau CDN géré par Adobe**.
**[Noms de domaine personnalisés avec réseau CDN géré par le client ou la cliente](./custom-domain-names-with-customer-managed-cdn.md)** : dans ce tutoriel, vous apprenez à ajouter un nom de domaine personnalisé à un **site web AEM as a Cloud Service avec réseau CDN géré par le client ou la cliente**.
