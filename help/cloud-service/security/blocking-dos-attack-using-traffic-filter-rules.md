---
title: Bloquer des attaques DoS et DDoS à l’aide de règles de filtrage du trafic
description: Découvrez comment bloquer des attaques DoS et DDoS à l’aide de règles de filtrage du trafic sur le réseau de diffusion de contenu fourni par AEM as a Cloud Service.
version: Cloud Service
feature: Security, Operations
topic: Security, Administration, Performance
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
duration: 436
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15184
thumbnail: KT-15184.jpeg
exl-id: 60c2306f-3cb6-4a6e-9588-5fa71472acf7
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1918'
ht-degree: 100%

---

# Bloquer des attaques DoS et DDoS à l’aide de règles de filtrage du trafic

Découvrez comment bloquer des attaques par déni de service (DoS) et par déni de service distribué (DDoS) à l’aide de règles de règles de **filtrage du trafic limitant le débit** et d’autres stratégies sur le réseau de diffusion de contenu géré par AEM as a Cloud Service (AEMCS). Ces attaques provoquent des pics de trafic sur le réseau de diffusion de contenu et potentiellement sur le service de publication AEM (soit l’origine) et peuvent avoir un impact sur la réactivité et la disponibilité du site.

Ce tutoriel sert de guide pour savoir _comment analyser vos modèles de trafic et configurer des [règles de filtrage du trafic limitant le débit](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf)_ pour réduire l’impact de ces attaques. Le tutoriel décrit également comment [configurer des alertes](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) afin de vous avertir en cas de suspicion d’attaque.

## Présentation des protections

Examinons les protections par défaut contre les attaques DDoS de votre site web AEM :

- **Mise en cache :** avec de bonnes politiques de mise en cache, l’impact d’une attaque DDoS est plus limité, car le réseau de diffusion de contenu empêche la plupart des demandes d’accéder au point d’origine et d’entraîner une dégradation des performances.
- **Mise à l’échelle automatique :** les services de création et de publication d’AEM adaptent automatiquement leur échelle pour gérer les pics de trafic, mais des augmentations soudaines et massives du trafic peuvent quand même les affecter.
- **Blocage :** le réseau de diffusion de contenu Adobe bloque le trafic vers le point d’origine s’il dépasse un débit défini par Adobe à partir d’une adresse IP spécifique, par PoP (Point de présence) du réseau de diffusion de contenu.
- **Alertes :** le Centre d’actions envoie une notification d’alerte de pic de trafic au point d’origine lorsque le trafic dépasse un certain débit. Cette alerte se déclenche lorsque le trafic sur un réseau de diffusion de contenu donné dépasse une valeur de taux de requêtes par adresse IP _définie par Adobe_. Voir [Alertes des règles de filtrage de trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) pour plus d’informations.

Ces protections intégrées doivent être considérées comme une référence pour la capacité d’une organisation à minimiser l’impact d’une attaque DDoS sur les performances. Chaque site web ayant des caractéristiques de performances différentes et pouvant ainsi subir une dégradation des performances avant que la limite de débit définie par Adobe ne soit atteinte, il est recommandé d’étendre les protections par défaut via la _configuration des clientes et clients_.

Voici quelques autres mesures recommandées que les clientes et clients peuvent prendre pour protéger leurs sites contre les attaques DDoS :

- Déclarer des **règles de filtrage du trafic limitant le débit** pour bloquer le trafic qui dépasse un certain débit à partir d’une seule adresse IP, par PoP. Il s’agit généralement d’un seuil inférieur à la limite de débit définie par Adobe.
- Configurer des **alertes** pour les règles de filtrage du trafic limitant le débit via une « action d’alerte ». Ainsi, lorsque la règle se déclenche, une notification est envoyée par le Centre d’actions.
- Augmenter la couverture du cache en déclarant des **transformations de requêtes** pour ignorer des paramètres de requête.

>[!NOTE]
>
>La fonctionnalité [alertes de règles de filtrage du trafic](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf#traffic-filter-rules-alerts) n’a pas encore été publiée. Pour en obtenir l’accès par le biais du programme d’adoption précoce, envoyez un e-mail à **<aemcs-waf-adopter@adobe.com>**.

### Variantes des règles de filtrage du trafic limitant le débit {#rate-limit-variations}

Il existe deux variantes des règles de filtrage du trafic limitant le débit :

1. Edge : bloque les requêtes en fonction du débit de la totalité du trafic (y compris celui qui peut être issu du cache du réseau de diffusion de contenu), pour une adresse IP donnée, par PoP.
1. Origine : bloque les requêtes en fonction du débit du trafic à destination de l’origine, pour une adresse IP donnée, par PoP.

## Parcours des clientes et clients

Les étapes ci-dessous illustrent le processus possible que les clientes et les clients doivent suivre pour protéger leurs sites web.

1. Reconnaissez la nécessité d’une règle de filtrage du trafic limitant le débit. Cela peut être dû à une réception de l’alerte Adobe intégrée de pic de trafic au niveau de l’origine, ou il peut s’agir d’une décision proactive de prendre des précautions pour réduire le risque d’un DDoS.
1. Analysez les modèles de trafic à l’aide d’un tableau de bord, si votre site est déjà actif, afin de déterminer les seuils optimaux pour vos règles de filtrage du trafic limitant le débit. Si votre site n’est pas encore actif, sélectionnez des valeurs en fonction de votre estimation du trafic.
1. À l’aide des valeurs de l’étape précédente, configurez les règles de filtrage du trafic limitant le débit. Activez les alertes correspondantes afin de recevoir une notification dès que le seuil est atteint.
1. Recevez des alertes de filtrage du trafic à chaque fois que des pics de trafic se produisent, ce qui vous permet de savoir si votre entreprise est potentiellement ciblée par des menaces malveillantes.
1. Agissez en réponse à l’alerte si nécessaire. Analysez le trafic pour déterminer si le pic reflète des requêtes légitimes plutôt qu’une attaque. Augmentez les seuils si le trafic est légitime ou réduisez-les si ce n’est pas le cas.

Le reste de ce tutoriel vous guide tout au long de ce processus.

## Reconnaître la nécessité de configurer des règles {#recognize-the-need}

Comme précisé précédemment, Adobe bloque par défaut le trafic sur le réseau de diffusion de contenu si ce trafic dépasse un certain débit. Cependant, certains sites web peuvent subir une dégradation de leurs performances en dessous de ce seuil. Par conséquent, les règles de filtrage du trafic limitant le débit doivent être configurées.

Idéalement, vous devez configurer les règles avant de passer en production. Dans la pratique, de nombreuses organisations ne déclarent les règles que de manière réactive lorsqu’elles sont alertées d’un pic de trafic indiquant une attaque probable.

Adobe envoie une alerte de pic de trafic à l&#39;origine sous la forme d’une [Notification du Centre d’actions](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/operations/actions-center) lorsque le seuil de trafic par défaut d’une adresse IP unique est dépassé, pour un PoP donné. Si vous avez reçu une telle alerte, il est recommandé de configurer une règles de filtrage du trafic limitant le débit. Cette alerte par défaut est différente des alertes que les clientes et les clients doivent activer explicitement lorsque ceux-ci définissent des règles de filtrage du trafic. Cela vous sera détaillé dans une section ultérieure.


## Analyser les modèles de trafic {#analyze-traffic}

Si votre site est déjà actif, vous pouvez analyser les modèles de trafic à l’aide des journaux du réseau de diffusion de contenu et de l’une des méthodes suivantes :

### ELK - configurer les outils du tableau de bord

Les outils de tableau de bord **Elasticsearch, Logstash et Kibana (ELK)** fournis par Adobe peuvent être utilisés pour analyser les journaux du réseau de diffusion de contenu. Ces outils incluent un tableau de bord qui affiche les modèles de trafic, qui aide à déterminer les seuils optimaux pour vos règles de filtrage du trafic limitant le débit.

- Clonez le référentiel GitHub [AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool).
- Configurez les outils en suivant la procédure [Configurer le conteneur ELK Docker](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool?tab=readme-ov-file#how-to-set-up-the-elk-docker-container).
- Dans le cadre de la configuration, importez le fichier `traffic-filter-rules-analysis-dashboard.ndjson` pour visualiser les données. Le tableau de bord _Trafic du réseau de diffusion de contenu_ inclut des visualisations qui affichent le nombre maximal de requêtes par adresse IP/PoP au niveau de l’origine et de la périphérie du réseau de diffusion de contenu.
- À partir de la vignette _Environnements_ de [Cloud Manager](https://my.cloudmanager.adobe.com/), téléchargez les journaux du réseau de diffusion de contenu du service de publication d’AEMCS.

  ![Téléchargements des journaux de réseau CDN Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 5 minutes peuvent s’écouler avant l’affichage des nouvelles requêtes dans les journaux du réseau CDN.

### Splunk - configurer les outils du tableau de bord

Les clientes et les clients qui ont activé le [Transfert de journal Splunk](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/implementing/developing/logging#splunk-logs) peuvent créer un tableau de bord pour analyser les modèles de trafic. Le fichier XML suivant vous aide à créer un tableau de bord sur Splunk :

- [Réseau de diffusion de contenu - Tableau de bord du trafic](./assets/traffic-dashboard.xml) : ce tableau de bord fournit des informations sur les modèles de trafic à l’origine et Edge du réseau de diffusion de contenu. Il inclut des visualisations qui affichent le nombre maximal de requêtes par adresse IP/PoP pour l’origine et Edge du réseau de diffusion de contenu.

### Observer les données

Les visualisations suivantes sont disponibles dans les tableaux de bord ELK et Splunk :

- **Requêtes par seconde (RPS) Edge par adresse IP de client et PoP** : cette visualisation affiche le nombre maximal de requêtes par adresse IP/PoP **pour Edge du réseau de diffusion de contenu**. Le pic sur la visualisation indique le nombre maximal de requêtes.

  **Tableau de bord ELK** :
  ![Tableau de bord ELK - Nombre maximum de requêtes par adresse IP/PoP](./assets/elk-edge-max-per-ip-pop.png)

  **Tableau de bord Splunk** :\
  ![Tableau de bord Splunk - Nombre maximum de requêtes par adresse IP/PoP](./assets/splunk-edge-max-per-ip-pop.png)

- **Requêtes par seconde (RPS) à l’origine par adresse IP du client et PoP** : cette visualisation affiche le nombre maximal de requêtes par adresse IP/PoP **à l’origine**. Le pic sur la visualisation indique le nombre maximal de requêtes.

  **Tableau de bord ELK** :
  ![Tableau de bord ELK - Nombre maximum de requêtes à l’origine par adresse IP/PoP](./assets/elk-origin-max-per-ip-pop.png)

  **Tableau de bord Splunk** :
  ![Tableau de bord Splunk - Nombre maximum de requêtes à l’origine par adresse IP/PoP](./assets/splunk-origin-max-per-ip-pop.png)

## Choix des valeurs de seuil

Les valeurs de seuil pour les règles de filtrage du trafic limitant le débit doivent être basées sur l’analyse ci-dessus et s’assurer que le trafic légitime n’est pas bloqué. Consultez le tableau suivant pour savoir comment choisir les valeurs de seuil :

| Variante | Valeur |
| :--------- | :------- |
| Origine | Prenez la valeur la plus élevée du nombre maximum de requêtes à l’origine par adresse IP/PoP dans des conditions **normales** de trafic (c’est-à-dire, pas lors d’un DDoS) et multipliez-la. |
| Edge | Prenez la valeur la plus élevée du nombre maximum de requêtes Edge par adresse IP/PoP dans des conditions **normales** de trafic (c’est-à-dire, pas lors d’un DDoS) et multipliez-la. |

Le facteur de multiplication à utiliser dépend des pics de trafic normaux auxquels vous vous attendez, en raison du trafic organique, de campagnes et d’autres événements. Un facteur de multiplication compris entre 5 et 10 est une option raisonnable.

Si votre site n’est pas encore en ligne, il n’y a aucune donnée à analyser. Vous devez estimer raisonnablement les valeurs appropriées à définir pour vos règles de filtrage du trafic limitant le débit. Par exemple :

| Variante | Valeur |
|------------------------------ |:-----------:|
| Edge | 500 |
| Origine | 100 |

## Configurer des règles {#configure-rules}

Configurez les règles de **filtrage du trafic limitant le débit** de votre fichier de projet AEM `/config/cdn.yaml` avec des valeurs basées sur l’explication ci-dessus. Le cas échéant, consultez votre équipe de sécurité web pour vous assurer que les valeurs de limites de débit sont appropriées et ne bloquent pas le trafic légitime.

Pour plus d’informations, consultez la section [Créer des règles dans votre projet AEM](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#create-rules-in-your-aem-project).

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    ...
    #  Prevent attack at edge by blocking client for 5 minutes if they make more than 500 requests per second on average
      - name: prevent-dos-attacks-edge
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 500 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: all # count all requests
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true
    #  Prevent attack at origin by blocking client for 5 minutes if they make more than 100 requests per second on average            
      - name: prevent-dos-attacks-origin
        when:
          reqProperty: tier
          in: ["author","publish"]
        rateLimit:
          limit: 100 # replace with the appropriate value
          window: 10 # compute the average over 10s
          penalty: 300 # block IP for 5 minutes
          count: fetches # count only fetches
          groupBy:
            - reqProperty: clientIp
        action: 
          type: log
          experimental_alert: true   
          
```

Notez que les règles d’origine et Edge sont déclarées et que la propriété alert est définie sur `true`, vous permettant de recevoir des alertes chaque fois que le seuil est atteint, ce qui indique probablement une attaque.

>[!NOTE]
>
>Le préfixe _experimental_ devant la fonction « experimental_alert » sera supprimé lorsque la fonctionnalité d’alerte sera publiée. Pour rejoindre le programme d’adoption précoce, envoyez un e-mail à **<aemcs-waf-adopter@adobe.com>**.

Il est recommandé de définir le type d’action de manière à pouvoir surveiller le trafic pendant quelques heures ou quelques jours, afin de s’assurer que le trafic légitime ne dépasse pas ces débits. Au bout de quelques jours, passez en mode bloc.

Pour déployer les modifications dans votre environnement AEMCS, procédez comme suit :

- Validez et envoyez les modifications au référentiel Git de Cloud Manager.
- Déployez les modifications dans l’environnement AEMCS à l’aide du pipeline de configuration de Cloud Manager. Voir [Déployer des règles via Cloud Manager](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) pour plus d’informations.
- Pour vérifier que la **règle de filtrage du trafic limitant le débit** fonctionne comme prévu, vous pouvez simuler une attaque comme décrit dans la section [Simulation d’attaque](#attack-simulation). Limitez le nombre de requêtes à une valeur supérieure à la valeur limite de débit définie dans la règle.

### Configurer des règles de transformation de requêtes {#configure-request-transform-rules}

En plus des règles de filtrage du trafic limitant le débit, il est recommandé d’utiliser des [transformations de requêtes](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#request-transformations) pour annuler des paramètres de requête dont l’application n’a pas besoin afin de réduire les possibilités de contourner le cache par le biais de techniques d’actualisation forcée du cache. Par exemple, si vous souhaitez autoriser uniquement les paramètres de requête `search` et `campaignId`, la règle suivante peut être déclarée :

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: 
    - dev
    - stage
    - prod  
data:  
  experimental_requestTransformations:
    rules:            
      - name: unset-all-query-params-except-those-needed
        when:
          reqProperty: tier
          in: ["publish"]
        actions:
          - type: unset
            queryParamMatch: ^(?!search$|campaignId$).*$
```

## Recevoir des alertes sur les règles de filtrage du trafic {#receiving-alerts}

Comme mentionné ci-dessus, si la règle de filtrage du trafic inclut *experimental_alert: true*, une alerte est reçue lorsque la règle est satisfaite.

## Agir en réponse aux alertes {#acting-on-alerts}

Parfois, l’alerte est à titre indicatif, ce qui vous permet d’avoir une idée de la fréquence des attaques. Il est utile d’analyser vos données de réseau de diffusion de contenu à l’aide du tableau de bord décrit ci-dessus, afin de vérifier que le pic de trafic est dû à une attaque et non à une simple augmentation du volume du trafic légitime. S’il s’agit de ce dernier cas, envisagez d’augmenter votre seuil.

## Simulation d’attaque{#attack-simulation}

Cette section décrit les méthodes de simulation d’une attaque par déni de service, qui peuvent être utilisées pour générer des données pour les tableaux de bord utilisés dans ce tutoriel et pour vérifier que toutes les règles configurées bloquent correctement les attaques.

>[!CAUTION]
>
> N’effectuez pas ces étapes dans un environnement de production. Les étapes suivantes sont destinées uniquement à des fins de simulation.
> 
>Si vous avez reçu une alerte indiquant un pic de trafic, passez à la section [Analyse des modèles de trafic](#analyzing-traffic-patterns).

Pour simuler une attaque, des outils tels qu’[Apache Benchmark](https://httpd.apache.org/docs/2.4/programs/ab.html), [Apache JMeter](https://jmeter.apache.org/), [Vegeta](https://github.com/tsenart/vegeta), ou autres, peuvent être utilisés.

### Requêtes Edge

En utilisant la commande [Vegeta](https://github.com/tsenart/vegeta) suivante, vous pouvez envoyer de nombreuses requêtes à votre site web :

```shell
$ echo "GET https://<YOUR-WEBSITE-DOMAIN>" | vegeta attack -rate=120 -duration=5s | vegeta report
```

Cette commande effectue 120 requêtes pendant 5 secondes et génère un rapport. En supposant que le débit du site web ne soit pas limité, cela peut entraîner un pic de trafic.

### Requêtes à l’origine

Pour contourner le cache du réseau de diffusion de contenu et envoyer des requêtes à l’origine (service de publication d’AEM), vous pouvez ajouter un paramètre de requête unique à l’URL. Reportez-vous à l’exemple de script Apache JMeter de la section [Simuler une attaque DoS à l’aide du script JMeter](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection#simulate-dos-attack-using-jmeter-script).

