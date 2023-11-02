---
title: Utiliser ModSecurity pour protéger votre site AEM des attaques par déni de service (DoS)
description: Découvrez comment activer ModSecurity pour protéger votre site contre les attaques par déni de service (DoS) à l’aide de l’ensemble de règles de base (CRS) OWASP ModSecurity.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
kt: 10385
thumbnail: KT-10385.png
doc-type: article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '1252'
ht-degree: 100%

---

# Utiliser ModSecurity pour protéger votre site AEM des attaques par déni de service (DoS)

Découvrez comment activer ModSecurity pour protéger votre site des attaques par déni de service (DoS) à l’aide de l’ **ensemble de règles de base (CRS) OWASP ModSecurity** sur le Dispatcher de publication d’Adobe Experience Manager (AEM).


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## Vue d’ensemble

La fondation [Open Web Application Security Project® (OWASP).](https://owasp.org/) fournit le [**Top 10 d’OWASP**](https://owasp.org/www-project-top-ten/) présentant les dix préoccupations de sécurité les plus critiques pour les applications web.

ModSecurity est une solution open source sur plusieurs plateformes qui offre une protection contre un éventail d’attaques à l’encontre des applications web. Elle permet également la surveillance du trafic HTTP, la journalisation et l’analyse en temps réel.

OWSAP® fournit également l’[ensemble de règles de base (CRS) OWASP® ModSecurity](https://github.com/coreruleset/coreruleset). Le CRS est un ensemble de règles de **détection d’attaque** génériques à utiliser avec ModSecurity. Ainsi, le CRS vise à protéger les applications web d’un grand nombre d’attaques, y compris celles du top 10 d’OWASP, avec un minimum de fausses alertes.

Ce tutoriel explique comment activer et configurer la règle CRS de **Protection de déni de service** pour protéger votre site contre une attaque potentielle par déni de service.

>[!TIP]
>
>Il est important de noter que le [réseau CDN géré](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr) d’AEM as a Cloud Service répond à la plupart des exigences de performances et de sécurité des clientes et clients. Cependant, ModSecurity fournit un niveau supplémentaire de sécurité et permet des règles et des configurations spécifiques aux clientes et clients.

## Ajouter le CRS au module de projet Dispatcher

1. Téléchargez et extrayez le [dernier ensemble de règles de base OWASP ModSecurity](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. Créez les dossiers `modsec/crs` dans `dispatcher/src/conf.d/` dans le code de votre projet AEM. Par exemple, dans la copie locale du [projet AEM Sites WKND](https://github.com/adobe/aem-guides-wknd).

   ![Dossier CRS dans le code de projet AEM - ModSecurity.](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. Copiez le dossier `coreruleset-X.Y.Z/rules` du package de version CRS téléchargé dans le dossier `dispatcher/src/conf.d/modsec/crs`.
1. Copiez le fichier `coreruleset-X.Y.Z/crs-setup.conf.example` du package de version CRS téléchargé dans le dossier `dispatcher/src/conf.d/modsec/crs` et renommez-le `crs-setup.conf`.
1. Désactivez toutes les règles CRS copiées à partir du `dispatcher/src/conf.d/modsec/crs/rules` en les renommant `XXXX-XXX-XXX.conf.disabled`. Vous pouvez utiliser les commandes ci-dessous pour renommer tous les fichiers en même temps.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   Voir Règles CRS et fichier de configuration renommés dans le code de projet WKND.

   ![Règles CRS désactivées dans le code de projet AEM - ModSecurity.](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## Activer et configurer la règle de protection de déni de service (DoS)

Pour activer et configurer la règle de protection de déni de service (DoS), procédez comme suit :

1. Activez la règle de protection de déni de service en renommant `REQUEST-912-DOS-PROTECTION.conf.disabled` en `REQUEST-912-DOS-PROTECTION.conf` (ou supprimez `.disabled` à partir de l’extension de nom de règle) au sein du dossier `dispatcher/src/conf.d/modsec/crs/rules`.
1. Configurez la règle en définissant les variables **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT**.
   1. Créez un fichier `crs-setup.custom.conf` dans le dossier `dispatcher/src/conf.d/modsec/crs`.
   1. Ajoutez l’extrait de code de règle ci-dessous au fichier nouvellement créé.

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

Dans cet exemple de configuration de règle, **DOS_COUNTER_THRESHOLD** est de 25, **DOS_BURST_TIME_SLICE** est de 60 secondes, et le délai d’expiration de **DOS_BLOCK_TIMEOUT** est de 600 secondes. Cette configuration identifie que plus de deux occurrences de 25 requêtes, à l’exception des fichiers statiques, en l’espace de 60 secondes sont considérées comme une attaque par déni de service, ce qui entraîne le blocage du client ou de la cliente effectuant la requête pendant 600 secondes (ou 10 minutes).

>[!WARNING]
>
>Pour définir les valeurs appropriées à vos besoins, collaborez avec votre équipe de sécurité web.

## Initialiser le CRS

Pour initialiser le CRS, supprimer les faux positifs courants et ajouter des exceptions locales pour votre site, procédez comme suit :

1. Pour initialiser le CRS, supprimez `.disabled` du fichier **REQUEST-901-INITIALIZATION**. En d’autres termes, renommez le fichier `REQUEST-901-INITIALIZATION.conf.disabled` en `REQUEST-901-INITIALIZATION.conf`.
1. Pour supprimer les faux positifs courants tels que le ping de l’adresse IP locale (127.0.0.1), supprimez `.disabled` du fichier **REQUEST-905-COMMON-EXCEPTIONS**.
1. Pour ajouter des exceptions locales telles que la plateforme AEM ou des chemins spécifiques à votre site, renommez `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` en `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. Ajoutez des exceptions de chemin d’accès spécifiques à la plateforme AEM au fichier nouvellement renommé.

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. Supprimez également `.disabled` de **REQUEST-910-IP-REPUTATION.conf.disabled** pour la vérification des blocs de réputation d’adresse IP et `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` pour vérifier le score d’anomalie.

>[!TIP]
>
>Lors de la configuration sur AEM 6.5, assurez-vous de remplacer les chemins ci-dessus par des chemins AMS ou On-Prem respectifs qui vérifient l’intégrité d’AEM (également appelés chemins de pulsation).

## Ajouter la configuration Apache ModSecurity

Pour activer ModSecurity (également appelé module Apache `mod_security`), procédez comme suit :

1. Créez `modsecurity.conf` sur `dispatcher/src/conf.d/modsec/modsecurity.conf` avec les configurations clés ci-dessous.

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. Sélectionnez le `.vhost` souhaité à partir du module de Dispatcher de votre projet AEM `dispatcher/src/conf.d/available_vhosts`, par exemple, `wknd.vhost`, ajoutez l’entrée ci-dessous en dehors du bloc `<VirtualHost>`.

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

Toutes les configurations _ModSecurity CRS_ et _DOS-PROTECTION_ ci-dessus sont disponibles sur la branche [tutoriel/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) du projet AEM Sites WKND pour vérification.

### Valider la configuration de Dispatcher

Lorsque vous utilisez AEM as a Cloud Service, avant de déployer vos modifications de la _configuration du Dispatcher_, il est recommandé de les valider localement à l’aide du script `validate` des [outils de Dispatcher du SDK AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=fr).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## Déployer

Déployez les configurations de Dispatcher validées localement à l’aide du pipeline [Niveau web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=fr) ou [Full stack](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?lang=fr#full-stack-code) de Cloud Manager. Vous pouvez également utiliser l’[environnement de développement rapide](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=fr) pour accélérer le délai d’exécution.

## Vérifier

Pour vérifier la protection de déni de service, dans cet exemple, envoyons plus de 50 requêtes (seuil de 25 requêtes multiplié par deux occurrences) en l’espace de 60 secondes. Toutefois, ces requêtes doivent passer par l’[intégration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr) d’AEM as a Cloud Service ou tout [autre réseau CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=fr#point-to-point-CDN) devant votre site web.

Une technique permettant d’assurer le passage du réseau CDN consiste à ajouter un paramètre de requête avec une **nouvelle valeur aléatoire pour chaque requête de page de site**.

Pour déclencher un plus grand nombre de requêtes (50 ou plus) sur une courte période (par exemple, 60 secondes), [JMeter](https://jmeter.apache.org/) ou l’[outil AB ou de référence](https://httpd.apache.org/docs/2.4/programs/ab.html) d’Apache peuvent être utilisés.

### Simuler une attaque par déni de service à l’aide du script JMeter

Pour simuler une attaque par déni de service à l’aide de JMeter, procédez comme suit :

1. [Téléchargez Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) et [installez](https://jmeter.apache.org/usermanual/get-started.html#install)-le localement
1. [Exécutez](https://jmeter.apache.org/usermanual/get-started.html#running)-le à l’aide du script `jmeter` depuis le répertoire `<JMETER-INSTALL-DIR>/bin`.
1. Ouvrez l’exemple de script JMX [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) dans JMeter à l’aide du menu d’outils **Ouvrir**.

   ![Ouverture d’un exemple de script de test JMX d’attaque par déni de service WKND - ModSecurity.](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. Mettez à jour la valeur de champ **Nom du serveur ou Adresse IP** dans l’échantillonage de requête HTTP _Page d’accueil_ et _Page d’aventure_ correspondant à l’URL de votre environnement de test AEM. Consultez d’autres détails de l’exemple de script JMeter.

   ![Requête HTTP du nom de serveur AEM JMeter - ModSecurity.](assets/modsecurity-crs/aem-server-name-http-request.png)

1. Exécutez le script en appuyant sur le bouton **Démarrer** dans le menu d’outils. Le script envoie 50 requêtes HTTP (5 profils utilisateurs et 10 itérations de boucle) contre la _Page d’accueil_ et la _Page d’aventure_ du site WKND. Par conséquent, un total de 100 requêtes de fichiers non statiques qualifie l’attaque du déni de service par la configuration de règle CRD personnalisée **DOS-PROTECTION**.

   ![Exécution du script JMeter - ModSecurity.](assets/modsecurity-crs/execute-jmeter-script.png)

1. Le listener JMeter **Affichage des résultats dans le tableau** affiche le statut de réponse **En échec** pour les numéros de requête supérieurs à 53 environ.

   ![Échec de la réponse dans l’affichage des résultats dans le tableau JMeter - ModSecurity.](assets/modsecurity-crs/failed-response-jmeter.png)

1. Le **code de réponse HTTP 503** est renvoyé pour les requêtes en échec, vous pouvez afficher les détails à l’aide du listener JMeter **Afficher l’arborescence des résultats**.

   ![Réponse 503 JMeter - ModSecurity.](assets/modsecurity-crs/503-response-jmeter.png)

### Journaux de révision

La configuration du journal ModSecurity consigne les détails de l’incident d’attaque DoS. Pour afficher les détails, procédez comme suit :

1. Téléchargez et ouvrez le fichier journal `httpderror` du **Dispatcher de l’instance de publication**.
1. Recherchez le mot `burst` dans le fichier journal, pour afficher les lignes d’**erreur**.

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. Consultez les détails comme _adresse IP du client_, action, message d’erreur et détails de la requête.

## Impact sur les performances de ModSecurity

L’activation de ModSecurity et des règles associées a des répercussions sur les performances. N’oubliez donc pas quelles règles sont requises, redondantes et ignorées. Associez-vous avec les personnes spécialisées chargées de votre sécurité web pour activer et personnaliser les règles CRS.

### Règles supplémentaires

Ce tutoriel active et personnalise uniquement la règle CRS **DOS-PROTECTION** à des fins de démonstration. Il est recommandé de collaborer avec des personnes spécialisées en sécurité web pour comprendre, examiner et configurer les règles appropriées.
