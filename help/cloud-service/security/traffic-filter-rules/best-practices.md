---
title: Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF
description: Découvrez les bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 178
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '413'
ht-degree: 100%

---

# Bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF

Découvrez les bonnes pratiques relatives aux règles de filtrage du trafic, y compris les règles WAF. Il est important de noter que les bonnes pratiques décrites dans le présent article ne sont pas exhaustives et n’ont pas pour but de se substituer à vos propres politiques et procédures de sécurité.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Bonnes pratiques générales

- Consultez votre équipe de sécurité pour déterminer les règles appropriées à votre entreprise.
- Testez toujours les règles dans les environnements de développement avant de les déployer dans les environnements d’évaluation et de production.
- Lorsque vous créez et validez des règles, commencez toujours par le type `action` `log` pour vous assurer que la règle ne bloquera pas le trafic légitime.
- Pour certaines règles, la transition de `log` à `block` doit être uniquement basée sur l’analyse d’un trafic suffisant sur le site.
- Introduisez les règles de manière incrémentielle et pensez à impliquer vos équipes de test (contrôle qualité, performance, test de pénétration) dans le processus.
- Analysez régulièrement l’impact des règles grâce aux [Outils du tableau de bord](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). Selon le volume de trafic sur votre site, l’analyse sera effectuée tous les jours, toutes les semaines ou tous les mois.
- Pour bloquer le trafic malveillant décelé après l’analyse, ajoutez des règles supplémentaires. Par exemple, les adresses IP qui ont attaqué votre site.
- La création, le déploiement et l’analyse de règles doivent constituer un processus continu et itératif. Il ne s’agit aucunement d’une activité ponctuelle.

## Bonnes pratiques relatives aux règles de filtrage du trafic

Activez les règles de filtrage du trafic ci-dessous pour votre projet AEM. Toutefois, les valeurs souhaitées pour les propriétés `rateLimit` et `clientCountry` doivent être déterminées en collaboration avec votre équipe de sécurité.

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
>Pour votre environnement de production, consultez votre équipe de sécurité web afin de déterminer les valeurs appropriées pour `rateLimit`.

## Bonnes pratiques relatives aux règles WAF

Dès l’activation de votre licence WAF pour le programme, le trafic correspondant aux indicateurs WAF apparaît dans les graphiques et les journaux de requêtes, même si vous ne les avez pas déclarés dans une règle. Cela vous permet de toujours avoir conscience d’un nouveau trafic malveillant potentiel et de créer des règles selon vos besoins. Examinez les indicateurs WAF qui ne sont pas reflétés dans les règles déclarées et envisagez de les créer.

Tenez compte des règles WAF ci-dessous pour votre projet AEM. Toutefois, les valeurs souhaitées pour `action` et la propriété `wafFlags` doivent être déterminées en collaboration avec votre équipe responsable de la sécurité.

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

En conclusion, vous disposez grâce à ce tutoriel des connaissances et des outils nécessaires pour renforcer la sécurité de vos applications web dans Adobe Experience Manager as a Cloud Service (AEMCS). Grâce aux exemples de règles concrets et aux informations récoltées suite à l’analyse des résultats, vous pouvez protéger efficacement votre site web et vos applications.



