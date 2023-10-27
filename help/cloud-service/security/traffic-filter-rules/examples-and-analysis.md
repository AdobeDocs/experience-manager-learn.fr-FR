---
title: Exemples et analyse des résultats de règles de filtrage du trafic incluant des règles WAF
description: Découvrez les différentes règles de filtrage du trafic, y compris des exemples de règles WAF. Découvrez également comment analyser les résultats à l’aide AEM journaux CDN as a Cloud Service (AEMCS).
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 87266a250eb91a82cf39c4a87e8f0119658cf4aa
workflow-type: tm+mt
source-wordcount: '1512'
ht-degree: 1%

---


# Exemples et analyse des résultats de règles de filtrage du trafic incluant des règles WAF

Découvrez comment déclarer différents types de règles de filtrage du trafic et analyser les résultats à l’aide des journaux de réseau de diffusion de contenu et des outils de tableau de bord Adobe Experience Manager as a Cloud Service (AEMCS).

Dans cette section, vous allez explorer des exemples pratiques de règles de filtrage du trafic, y compris les règles WAF. Vous apprendrez à consigner, autoriser et bloquer les demandes en fonction de l’URI (ou du chemin), de l’adresse IP, du nombre de demandes et de différents types d’attaques à l’aide de la variable [AEM projet WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

De plus, vous découvrirez comment utiliser des outils de tableau de bord qui ingèrent des journaux de réseau de diffusion de contenu AEM pour visualiser des mesures essentielles au moyen de tableaux de bord d’Adobe fournis.

Pour vous aligner sur vos exigences spécifiques, vous pouvez améliorer et créer des tableaux de bord personnalisés, afin d’obtenir des informations plus approfondies et d’optimiser les configurations de règles pour vos sites AEM.

## Exemples

Explorons divers exemples de règles de filtrage du trafic, y compris les règles WAF. Assurez-vous d’avoir effectué le processus de configuration requis, comme décrit dans la section précédente. [configuration](./how-to-setup.md) et que vous avez cloné le [AEM projet WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project).

### Journalisation des requêtes

Commencer par **Journalisation des requêtes de connexion et de déconnexion WKND** par rapport au service de publication AEM.

- Ajoutez la règle suivante au fichier du projet WKND `/config/cdn.yaml` fichier .

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

- Validez et envoyez les modifications au référentiel Git de Cloud Manager.

- Déployer les modifications dans AEM environnement de développement à l’aide de Cloud Manager `Dev-Config` pipeline de configuration [créé précédemment](how-to-setup.md#deploy-rules-through-cloud-manager).

  ![Pipeline de configuration de Cloud Manager](./assets/cloud-manager-config-pipeline.png)

- Testez la règle en vous connectant et en se déconnectant du site WKND de votre programme sur le service de publication (par exemple, `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`). Vous pouvez utiliser `asmith/asmith` comme nom d’utilisateur et mot de passe.

  ![Connexion WKND](./assets/wknd-login.png)

#### Analyse de{#analyzing}

Analysons les résultats de la `publish-auth-requests` en téléchargeant les journaux de réseau de diffusion de contenu AEM à partir de Cloud Manager et en utilisant la variable [Outils du tableau de bord](how-to-setup.md#analyze-results-using-elk-dashboard-tool), que vous avez configuré dans le chapitre précédent.

- De [Cloud Manager](https://my.cloudmanager.adobe.com/)&#39;s **Environnements** carte, télécharger AEMCS **Publier** journaux CDN du service.

  ![Téléchargements des journaux CDN de Cloud Manager](./assets/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  >    Les nouvelles requêtes peuvent prendre jusqu’à 5 minutes pour s’afficher dans les journaux du réseau de diffusion de contenu.

- Copiez le fichier journal téléchargé (par exemple : `publish_cdn_2023-10-24.log` dans la capture d’écran ci-dessous) dans la fonction `logs/dev` dossier du projet d’outil Elastic dashboard.

  ![Dossier des journaux d’outils ELK](./assets/elk-tool-logs-folder.png){width="800" zoomable="yes"}

- Actualisez la page de l’outil de tableau de bord Elastic .
   - En haut **Filtre global** , modifiez la section `aem_env_name.keyword` filtrer et sélectionner la variable `dev` valeur d’environnement.

     ![Filtre global de l’outil ELK](./assets/elk-tool-global-filter.png)

   - Pour modifier l’intervalle, cliquez sur l’icône de calendrier dans le coin supérieur droit et sélectionnez l’intervalle de temps souhaité.

     ![Intervalle horaire de l’outil ELK](./assets/elk-tool-time-interval.png)

- Examinez les  **Requêtes analysées**, **Demandes marquées**, et **Détails des requêtes marquées** panneaux. Pour les entrées de journal CDN correspondantes, il doit afficher les valeurs de l’adresse IP du client (cli_ip) de chaque entrée (cli_ip), de l’hôte, de l’url, de l’action (waf_action) et du nom de la règle (waf_match).

  ![Tableau de bord de l’outil ELK](./assets/elk-tool-dashboard.png)


### Blocage des requêtes

Dans cet exemple, ajoutons une page dans une _internal_ dossier au chemin d’accès `/content/wknd/internal` dans le projet WKND déployé. Ensuite, déclarez une règle de filtre de trafic qui **bloque le trafic** vers des sous-pages de n’importe quel autre emplacement qu’une adresse IP spécifique qui correspond à votre entreprise (par exemple, un VPN d’entreprise).

Vous pouvez créer votre propre page interne (par exemple, `demo-page.html`) ou utilisez la variable [package joint](./assets/demo-internal-pages-package.zip).

- Ajoutez la règle suivante dans le fichier du projet WKND `/config/cdn.yaml` fichier :

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

- Validez et envoyez les modifications au référentiel Git de Cloud Manager.

- Déployez les modifications dans l’environnement de développement AEM à l’aide du [créé précédemment](how-to-setup.md#deploy-rules-through-cloud-manager) `Dev-Config` pipeline de configuration dans Cloud Manager.

- Testez la règle en accédant à la page interne du site WKND, par exemple `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` ou à l’aide de la commande CURL ci-dessous :

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- Répétez l’étape ci-dessus à partir de l’adresse IP que vous avez utilisée dans la règle, puis d’une autre adresse IP (en utilisant votre téléphone mobile, par exemple).

#### Analyse de

Pour analyser les résultats du `block-internal-paths` , suivez les mêmes étapes que celles décrites dans la section [exemple précédent](#analyzing).

Cependant, cette fois, vous devriez voir le **Demandes bloquées** et les valeurs correspondantes dans les colonnes IP du client (cli_ip), host, URL, action (waf_action) et rule-name (waf_match).

![Requête bloquée dans le tableau de bord de l’outil ELK](./assets/elk-tool-dashboard-blocked.png)


### Prévention des attaques DoS

Allons-y. **empêcher les attaques du DoS ;** en bloquant les requêtes d’une adresse IP, effectuant 100 requêtes par seconde, ce qui le bloque pendant 5 minutes.

- Ajoutez ce qui suit : [règle de filtre de trafic limite de taux](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#ratelimit-structure) dans le projet WKND `/config/cdn.yaml` fichier .

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
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block     
```

>[!WARNING]
>
>Pour votre environnement de production, collaborez avec votre équipe de sécurité web afin de déterminer les valeurs appropriées pour `rateLimit`,

- Validation, notification push et déploiement des modifications comme indiqué dans la section [exemples précédents](#logging-requests).

- Pour simuler l’attaque du DoS, utilisez ce qui suit : [Vegeta](https://github.com/tsenart/vegeta) .

  ```shell
  $ echo "GET https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html" | vegeta attack -rate=120 -duration=5s | vegeta report
  ```

  Cette commande effectue 120 demandes pendant 5 secondes et génère un rapport. Comme vous pouvez le constater, le taux de réussite est de 32,5 % ; un code de réponse HTTP 406 est reçu pour le reste, ce qui démontre que le trafic a été bloqué.

  ![Attaque contre la Vegeta DoS](./assets/vegeta-dos-attack.png)

#### Analyse de

Pour analyser les résultats du `prevent-dos-attacks` , suivez les mêmes étapes que celles décrites dans la section [exemple précédent](#analyzing).

Cette fois, vous devriez voir beaucoup de **Demandes bloquées** et les valeurs correspondantes dans les colonnes IP du client (cli_ip), host, url, action (waf_action) et rule-name (waf_match).

![Requête de déni de service dans le tableau de bord de l’outil ELK](./assets/elk-tool-dashboard-dos.png)

En outre, la variable **Les 100 plus grandes attaques par adresse IP du client, pays et agent-utilisateur** Les panneaux affichent des détails supplémentaires, qui peuvent être utilisés pour optimiser davantage la configuration des règles.

![Tableau de bord de l’outil ELK - 100 premières requêtes](./assets/elk-tool-dashboard-dos-top-100.png)

### Règles WAF

Jusqu’à présent, tous les clients Sites et Forms peuvent configurer les exemples de règles de filtrage du trafic.

Examinons ensuite l’expérience d’un client qui a acquis une licence de protection améliorée ou WAF-DDoS, qui lui permet de configurer des règles avancées pour protéger les sites web contre des attaques plus sophistiquées.

Avant de poursuivre, activez la protection WAF-DDoS pour votre programme, comme décrit dans la documentation des règles de filtrage du trafic . [étapes de configuration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html?lang=en#setup).

#### Sans WAFFlags

Commençons par voir l’expérience avant même que les règles WAF ne soient déclarées. Lorsque le WAF-DDoS est activé sur votre programme, votre réseau de diffusion de contenu consigne par défaut toutes les correspondances de trafic malveillant. Vous disposez donc des informations appropriées pour trouver les règles appropriées.

Commençons par attaquer le site WKND sans ajouter de règle WAF (ou par utiliser la fonction `wafFlags` ) et analyser les résultats.

- Pour simuler une attaque, utilisez la variable [Nikto](https://github.com/sullo/nikto) la commande ci-dessous, qui envoie environ 700 demandes malveillantes en 6 minutes.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

  ![Simulation des attaques de Nikto](./assets/nikto-attack.png)

  Pour en savoir plus sur la simulation des attaques, consultez la section [Nikto - Réglage de l’analyse](https://github.com/sullo/nikto/wiki/Scan-Tuning) documentation, qui vous indique comment spécifier le type d’attaques de test à inclure ou à exclure.

##### Analyse de

Pour analyser les résultats de la simulation d&#39;attaque, procédez comme décrit dans la section [exemple précédent](#analyzing).

Cependant, cette fois, vous devriez voir le **Demandes marquées** et les valeurs correspondantes dans les colonnes IP du client (cli_ip), host, url, action (waf_action) et rule-name (waf_match). Ces informations vous permettent d’analyser les résultats et d’optimiser le paramétrage des règles.

![Requête avec indicateur WAF dans le tableau de bord de l’outil ELK](./assets/elk-tool-dashboard-waf-flagged.png)

Notez comment la variable **Distribution des indicateurs WAF** et **Les principales attaques** Les panneaux affichent des détails supplémentaires, qui peuvent être utilisés pour optimiser davantage la configuration des règles.

![Tableau de bord de l’outil ELK - Demande d’accès aux indicateurs WAF](./assets/elk-tool-dashboard-waf-flagged-top-attacks-1.png)

![Tableau de bord de l’outil ELK Demande des principales attaques WAF](./assets/elk-tool-dashboard-waf-flagged-top-attacks-2.png)


#### Avec WAFFlags

Ajoutons maintenant une règle WAF qui contient `wafFlags` dans le cadre de la propriété `action` et **bloquer les demandes d&#39;attaque simulées ;**.

Toutefois, du point de vue de la syntaxe, les règles de la WAF sont similaires à celles que nous avons vues précédemment : `action` référence une ou plusieurs propriétés `wafFlags` valeurs. Pour en savoir plus sur la variable `wafFlags`, consultez la section [Liste des indicateurs WAF](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#waf-flags-list) .

- Ajoutez la règle suivante dans le fichier du projet WKND `/config/cdn.yaml` fichier . Notez comment la variable `block-waf-flags` règle inclut certains des wafFlags qui s’affichaient dans l’outil de tableau de bord lors d’une attaque avec un trafic malveillant simulé. En effet, il est recommandé au fil du temps d’analyser les journaux afin de déterminer les nouvelles règles à déclarer, à mesure que le paysage de la menace évolue.

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
    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - SIGSCI-IP
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8        
```

- Validation, notification push et déploiement des modifications comme indiqué dans la section [exemples précédents](#logging-requests).

- Pour simuler une attaque, utiliser la même méthode [Nikto](https://github.com/sullo/nikto) comme avant.

  ```shell
  $ ./nikto.pl -useragent "AttackSimulationAgent (Demo/1.0)" -D V -Tuning 9 -ssl -h https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html
  ```

##### Analyse de

Répétez les mêmes étapes que celles décrites dans la section [exemple précédent](#analyzing).

Cette fois, vous devriez voir des entrées sous **Demandes bloquées** et les valeurs correspondantes dans les colonnes IP du client (cli_ip), host, url, action (waf_action) et rule-name (waf_match).

![Requête bloquée WAF dans le tableau de bord de l’outil ELK](./assets/elk-tool-dashboard-waf-blocked.png)

En outre, la variable **Distribution des indicateurs WAF** et **Les principales attaques** les panneaux affichent des détails supplémentaires.

![Tableau de bord de l’outil ELK - Demande d’accès aux indicateurs WAF](./assets/elk-tool-dashboard-waf-blocked-top-attacks-1.png)

![Tableau de bord de l’outil ELK Demande des principales attaques WAF](./assets/elk-tool-dashboard-waf-blocked-top-attacks-2.png)

### Analyse exhaustive

Dans la section ci-dessus _analyse_ , vous avez appris à analyser les résultats de règles spécifiques à l’aide de l’outil de tableau de bord. Vous pouvez explorer plus en détail l’analyse des résultats à l’aide d’autres panneaux de tableau de bord, notamment :


- Requêtes analysées, marquées et bloquées
- Distribution des indicateurs WAF au fil du temps
- Règles de filtrage du trafic déclenchées au fil du temps
- Meilleures attaques par identifiant de drapeau de la WAF
- Filtre de trafic déclenché le plus haut
- 100 premiers attaquants par adresse IP du client, pays et agent-utilisateur

![Analyse complète du tableau de bord de l’outil ELK](./assets/elk-tool-dashboard-comprehensive-analysis-1.png)

![Analyse complète du tableau de bord de l’outil ELK](./assets/elk-tool-dashboard-comprehensive-analysis-2.png)


## Étape suivante

Familiariser avec les recommandations [bonnes pratiques](./best-practices.md) afin de réduire le risque de violations de sécurité.

## Ressources supplémentaires

[Syntaxe des règles de filtre de trafic](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#rules-syntax)

[Format de journal CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/traffic-filter-rules-including-waf.html#cdn-log-format)

