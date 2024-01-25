---
title: Journaux
description: Les journaux sont en première ligne pour le débogage des applications AEM dans AEM as a Cloud Service, mais dépendent d’une journalisation adéquate dans l’application AEM déployée.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 275
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 98%

---

# Déboguer AEM as a Cloud Service à l’aide de journaux

Les journaux sont en première ligne pour le débogage des applications AEM dans AEM as a Cloud Service, mais dépendent d’une journalisation adéquate dans l’application AEM déployée.

Toutes les activités de journal pour le service AEM d’un environnement donné (Création, Publication/Dispatcher de publication) sont consolidées dans un seul fichier journal, même si différentes capsules de ce service génèrent les instructions de journal.

Les ID de capsule sont fournis dans chaque instruction de journal, ce qui permet de filtrer ou de regrouper les instructions. Les ID de capsule sont au format suivant :

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Exemple : `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Fichiers journaux personnalisés

AEM as a Cloud Service ne prend pas en charge les fichiers journaux personnalisés, mais il prend en charge la journalisation personnalisée.

Pour que les journaux Java soient disponibles dans AEM as a Cloud Service (via [Cloud Manager](#cloud-manager) ou l’[interface de ligne de commande d’Adobe I/O](#aio)), les instructions de journal personnalisées doivent être écrites dans le `error.log`. Les journaux écrits dans des journaux dont le nom est personnalisé, tels que `example.log`, ne seront pas accessibles à partir d’AEM as a Cloud Service.

Les journaux peuvent être écrits dans le `error.log` à l’aide d’une propriété de configuration OSGi Sling LogManager dans les fichiers `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` de l’application.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Journaux du service de création et de publication d’AEM

À la fois le service de création et celui de publication d’AEM fournissent les journaux de serveur d’exécution AEM :

+ `aemerror` est le journal des erreurs Java (situé à l’adresse `/crx-quickstart/logs/error.log` sur le démarrage rapide local du SDK AEM). Voici les [niveaux de journal recommandés](#log-levels) pour les enregistreurs personnalisés par type d’environnement :
   + Développement : `DEBUG`
   + Évaluation : `WARN`
   + Production : `ERROR`
+ `aemaccess` répertorie les requêtes HTTP au service AEM avec des détails.
+ `aemrequest` répertorie les requêtes HTTP effectuées au service AEM et leur réponse HTTP correspondante.

## Journaux du Dispatcher de publication AEM

Seul le Dispatcher de publication AEM fournit les journaux du serveur web Apache et de Dispatcher, car ces aspects n’existent que dans le niveau de publication AEM et non dans celui de création.

+ `httpdaccess` répertorie les requêtes HTTP effectuées sur le serveur web Apache/Dispatcher du service AEM.
+ `httperror` répertorie les messages de journal du serveur web Apache et aide au débogage des modules Apache pris en charge, tels que `mod_rewrite`.
   + Développement : `DEBUG`
   + Évaluation : `WARN`
   + Production : `ERROR`
+ `aemdispatcher` répertorie les messages de journal des modules de Dispatcher, y compris le filtrage et la diffusion à partir des messages du cache.
   + Développement : `DEBUG`
   + Évaluation : `WARN`
   + Production : `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager permet le téléchargement des journaux par jour, via l’action Télécharger les journaux d’un environnement.

![Cloud Manager - Téléchargement de journaux.](./assets/logs/download-logs.png)

Ces journaux peuvent être téléchargés et inspectés à l’aide de n’importe quel outil d’analyse des journaux.

## Interface de ligne de commande Adobe I/O avec le plug-in Cloud Manager{#aio}

Adobe Cloud Manager prend en charge l’accès à AEM as a Cloud Service via l’[interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli) avec le [plug-in Cloud Manager pour l’interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Tout d’abord, [configurez l’interface de ligne de commande d’Adobe I/O avec le plug-in Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Assurez-vous que l’ID de programme et l’ID d’environnement appropriés ont été identifiés et utilisez [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) pour répertorier les options de journal utilisées pour [suivre](#aio-cli-tail-logs) ou [télécharger](#aio-cli-download-logs) les journaux.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Suivre les journaux{#aio-cli-tail-logs}

L’interface de ligne de commande d’Adobe I/O permet de suivre les journaux en temps réel à partir d’AEM as a Cloud Service à l’aide de la commande [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name). Le suivi est utile pour observer l’activité du journal en temps réel lorsque des actions sont effectuées sur l’environnement AEM as a Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

D’autres outils de ligne de commande, tels que `grep`, peuvent être utilisés avec `tail-logs` pour isoler les instructions de journal présentant un intérêt, par exemple :

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... affiche uniquement les instructions de journal générées à partir de `com.example.MySlingModel` ou qui contiennent cette chaîne.

### Télécharger les journaux{#aio-cli-download-logs}

L’interface de ligne de commande d’Adobe I/O permet de télécharger les journaux depuis AEM as a Cloud Service à l’aide de la commande [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days). Cela produit le même résultat final que le téléchargement des journaux à partir de l’interface utilisateur web de Cloud Manager, la différence étant que la commande `download-logs` regroupe les journaux par jour, en fonction du nombre de jours demandé.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Comprendre les journaux

Les journaux d’AEM as a Cloud Service contiennent plusieurs instructions de journal d’écriture de pods. Comme plusieurs instances AEM écrivent dans le même fichier journal, il est important de comprendre comment analyser et réduire le bruit pendant le débogage. L’extrait de code journal `aemerror` sert d’exemple :

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

À l’aide des ID de pod et du point de données après la date et l’heure, les journaux peuvent être regroupés par pod ou par instance AEM au sein du service, ce qui facilite le suivi et la compréhension de l’exécution du code.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Niveaux de journal recommandés{#log-levels}

Les instructions générales d’Adobe sur les niveaux de journalisation par environnement AEM as a Cloud Service sont les suivantes :

+ Développement local (SDK AEM) : `DEBUG`
+ Développement : `DEBUG`
+ Évaluation : `WARN`
+ Production : `ERROR`

La définition du niveau de journalisation approprié à chaque type d’environnement se fait avec AEM as a Cloud Service. Les niveaux de journalisation sont conservés dans le code.

+ Les configurations de journal Java sont conservées dans les configurations OSGi.
+ Les niveaux de journalisation du serveur web Apache et du Dispatcher sont conservés dans le projet du Dispatcher

Les niveaux de journalisation nécessitent par conséquent un déploiement pour changer.

### Variables spécifiques à l’environnement pour définir les niveaux de journalisation Java

Au lieu de définir des niveaux de journalisation Java statiques bien connus pour chaque environnement, vous pouvez utiliser les [variables spécifiques à l’environnement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=fr#environment-specific-configuration-values) d’AEM as a Cloud Service pour paramétrer les niveaux de journalisation, ce qui permet de modifier dynamiquement les valeurs via [l’interface de ligne de commande Adobe I/O avec le plug-in Cloud Manager](#aio-cli).

Cela nécessite la mise à jour des configurations OSGi de journalisation pour utiliser les espaces réservés de variable spécifiques à l’environnement. Les [valeurs par défaut](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=fr#default-values) pour les niveaux de journalisation doivent être définies conformément aux [Adobe Recommendations](#log-levels). Par exemple :

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Cette approche présente des inconvénients à prendre en compte :

+ [Un nombre limité de variables d’environnement est autorisé](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=fr#number-of-variables) et la création d’une variable pour gérer le niveau de journalisation en utilise une.
+ Les variables d’environnement peuvent être gérées par programmation via [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=fr), [Interface de ligne de commande Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), et [API HTTP Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=fr#cloud-manager-api-format-for-setting-properties).
+ Les modifications apportées aux variables d’environnement doivent être réinitialisées manuellement par un outil pris en charge. Oublier de réinitialiser un environnement à trafic élevé, tel que celui de production, à un niveau de journalisation moins détaillé peut inonder les journaux et affecter les performances d’AEM.

_Les variables spécifiques à l’environnement ne fonctionnent pas pour les configurations de journaux du serveur web Apache ou de Dispatcher, car elles ne sont pas configurées via la configuration OSGi._
