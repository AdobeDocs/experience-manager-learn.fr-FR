---
title: Surveillance des requêtes sensibles
description: Découvrez comment surveiller les requêtes sensibles en les enregistrant à l’aide des règles de filtrage du trafic dans AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18311
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 34%

---

# Surveillance des requêtes sensibles

Découvrez comment surveiller les requêtes sensibles en les enregistrant à l’aide des règles de filtrage du trafic dans AEM as a Cloud Service.

La journalisation vous permet d’observer les schémas de trafic sans affecter les utilisateurs finaux ou les services. Il s’agit d’une première étape cruciale avant d’implémenter des règles de blocage.

Ce tutoriel explique comment **consigner les requêtes des chemins de connexion et de déconnexion WKND** par rapport au service de publication AEM.

## Pourquoi et quand consigner les requêtes

La journalisation de requêtes spécifiques est une pratique à faible risque et à forte valeur ajoutée qui permet de comprendre comment les utilisateurs, et les acteurs potentiellement malveillants, interagissent avec votre application AEM. Il est particulièrement utile avant d’appliquer des règles de blocage, ce qui vous permet d’affiner votre posture de sécurité sans interrompre le trafic légitime.

Les scénarios courants de journalisation sont les suivants :

- Valider l’impact et la portée d’une règle avant de la promouvoir en mode `block`.
- Surveillance des chemins de connexion/déconnexion et des points d’entrée d’authentification pour détecter les modèles inhabituels ou les tentatives de force brute.
- Suivi de l’accès haute fréquence aux points d’entrée de l’API pour une utilisation abusive potentielle ou une activité DoS.
- Établir des références pour le comportement des robots avant d’appliquer des contrôles plus stricts.
- En cas d’incidents de sécurité, fournir des données médico-légales pour comprendre la nature de l’attaque et les ressources affectées.

## Prérequis

Avant de poursuivre, assurez-vous d’avoir terminé la configuration requise, comme décrit dans le tutoriel [Comment configurer le filtre de trafic et les règles de WAF](../setup.md). En outre, vous avez cloné et déployé le projet [Sites WKND AEM](https://github.com/adobe/aem-guides-wknd) dans votre environnement AEM.

## Exemple : consigner les demandes de connexion et de déconnexion WKND

Dans cet exemple, vous créez une règle de filtrage du trafic pour consigner les requêtes effectuées aux chemins de connexion et de déconnexion WKND sur le service de publication AEM. Il vous permet de surveiller les tentatives d’authentification et d’identifier les problèmes de sécurité potentiels.

- Ajoutez la règle suivante au fichier de projet WKND `/config/cdn.yaml`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
    - name: publish-auth-requests
      when:
        allOf:
          - reqProperty: tier
            matches: publish
          - reqProperty: path
            in:
              - /system/sling/login/j_security_check
              - /system/sling/logout
      action: log   
```

- Validez et envoyez les modifications au référentiel Git de Cloud Manager.

- Déployez les modifications dans l’environnement AEM à l’aide du pipeline de configuration Cloud Manager [créé précédemment](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Testez la règle en vous connectant et en vous déconnectant du site WKND de votre programme (par exemple, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Vous pouvez utiliser `asmith/asmith` comme nom d’utilisation et mot de passe.

  ![Connexion à WKND.](../assets/how-to/wknd-login.png)

## Analyser

Analysons les résultats de la règle `publish-auth-requests` en téléchargeant les journaux CDN AEMCS depuis Cloud Manager et en utilisant l’outil d’analyse de journal [CDN](../setup.md#setup-the-elastic-dashboard-tool).

- Cliquez sur la vignette **Environnements** de [Cloud Manager](https://my.cloudmanager.adobe.com/) et téléchargez les journaux du réseau CDN du service de **Publication** AEMCS.

  ![Téléchargements des journaux de réseau CDN Cloud Manager](../assets/how-to/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 5 minutes peuvent s’écouler avant l’affichage des nouvelles requêtes dans les journaux du réseau CDN.

- Copiez le fichier journal téléchargé (par exemple, `publish_cdn_2023-10-24.log` dans la copie d’écran ci-dessous) dans le dossier `logs/dev` du projet d’outil de tableau de bord Elastic.

  ![Dossier des journaux d’outils ELK.](../assets/how-to/elk-tool-logs-folder.png)

- Actualisez la page de l’outil de tableau de bord Elastic.
   - Dans la section supérieure **Filtre global**, modifiez le filtre `aem_env_name.keyword` et sélectionnez la valeur d’environnement `dev`.

     ![Filtre global de l’outil ELK.](../assets/how-to/elk-tool-global-filter.png)

   - Pour modifier l’intervalle de temps, cliquez sur l’icône de calendrier dans le coin supérieur droit et sélectionnez la valeur souhaitée.

     ![Intervalle de temps de l’outil ELK.](../assets/how-to/elk-tool-time-interval.png)

- Consultez les panneaux **Requêtes analysées**, **Requêtes marquées** et **Détails des requêtes marquées**. Pour les entrées de journal du réseau CDN correspondantes, les panneaux doivent afficher les valeurs d’adresse IP du client ou de la cliente (cli_ip), d’hôte, d’url, d’action (waf_action) et de nom de règle (waf_match) de chaque entrée.

  ![Tableau de bord de l’outil ELK.](../assets/how-to/elk-tool-dashboard.png)

