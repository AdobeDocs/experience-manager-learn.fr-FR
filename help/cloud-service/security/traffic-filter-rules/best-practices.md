---
title: Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF
description: Découvrez les bonnes pratiques recommandées pour les règles de filtre de trafic, y compris les règles WAF.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: 67e0a7530549a0d380e9ef82e3747c40d17b1b75
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---


# Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF

Découvrez les bonnes pratiques recommandées pour les règles de filtrage du trafic, y compris les règles WAF. Il est important de noter que les bonnes pratiques décrites dans le présent article ne sont pas exhaustives et n&#39;ont pas pour but de se substituer à vos propres politiques et procédures de sécurité.

## Bonnes pratiques générales

- Pour déterminer les règles appropriées à votre entreprise, collaborez avec votre équipe de sécurité.
- Toujours tester les règles dans les environnements de développement avant de les déployer dans les environnements d’évaluation et de production.
- Lorsque vous déclarez et validez des règles, commencez toujours par `action` type `log` pour s’assurer que la règle ne bloque pas le trafic légitime.
- Pour certaines règles, la transition de `log` to `block` doit être uniquement basé sur l’analyse d’un trafic suffisant sur le site.
- Introduisez des règles de manière incrémentielle et envisagez d’impliquer vos équipes de test (contrôle qualité, performance, test de pénétration) dans le processus.
- Analysez régulièrement l’impact des règles en utilisant la variable [Outils du tableau de bord](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). Selon le volume de trafic de votre site, l’analyse peut être effectuée tous les jours, toutes les semaines ou tous les mois.
- Pour bloquer le trafic malveillant que vous connaissez peut-être après l’analyse, ajoutez des règles supplémentaires. Par exemple, certaines adresses IP qui ont attaqué votre site.
- La création, le déploiement et l’analyse de règles doivent être un processus itératif continu. Il ne s’agit pas d’une activité ponctuelle.

## Bonnes pratiques relatives aux règles de filtrage du trafic

Activez les règles de filtrage du trafic ci-dessous pour votre projet AEM. Toutefois, les valeurs souhaitées pour `rateLimit` et `clientCountry` Les propriétés doivent être déterminées en collaboration avec votre équipe de sécurité.

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
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>Pour votre environnement de production, collaborez avec votre équipe de sécurité web afin de déterminer les valeurs appropriées pour `rateLimit`,

## Bonnes pratiques relatives aux règles WAF

Une fois que la méthode WAF est sous licence et activée pour votre programme, les indicateurs de trafic correspondant à la méthode WAF apparaissent dans les graphiques et les journaux de requêtes, même si vous ne les avez pas déclarés dans une règle. Cela vous permet de toujours être conscient d’un nouveau trafic malveillant potentiel et de créer des règles selon vos besoins. Examinez les indicateurs WAF qui ne sont pas reflétés dans les règles déclarées et envisagez de les déclarer.

Tenez compte des règles WAF ci-dessous pour votre projet AEM. Toutefois, les valeurs souhaitées pour `action` et `wafFlags` doit être déterminée en collaboration avec votre équipe de sécurité.

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

    # Traffic Filter rules shown in above section
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
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## Résumé

En conclusion, ce tutoriel vous a fourni les connaissances et les outils nécessaires pour renforcer la sécurité de vos applications web dans Adobe Experience Manager as a Cloud Service (AEMCS). Grâce à des exemples de règles pratiques et des informations sur l’analyse des résultats, vous pouvez protéger efficacement votre site web et vos applications.
