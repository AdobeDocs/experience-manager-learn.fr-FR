---
title: Journaux
description: Les journaux se trouvent en première ligne du débogage des applications AEM dans AEM as a Cloud Service, mais dépendent d’une journalisation adéquate dans l’application d’AEM déployée.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: 2685f2553349d6f0b48e03f2ed24dcea7ad9ac70
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 3%

---

# Débogage AEM as a Cloud Service à l’aide des journaux

Les journaux se trouvent en première ligne du débogage des applications AEM dans AEM as a Cloud Service, mais dépendent d’une journalisation adéquate dans l’application d’AEM déployée.

Toutes les activités de journal pour le service d’AEM d’un environnement donné (Auteur, Publier/Publier Dispatcher) sont consolidées dans un seul fichier journal, même si différents modules de ce service génèrent les instructions de journal.

Les ID de capsule sont fournis dans chaque instruction de journal, ce qui permet de filtrer ou de regrouper les instructions de journal. Les ID de capsule sont au format suivant :

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Exemple : `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Fichiers journaux personnalisés

AEM en tant que Cloud Services ne prend pas en charge les fichiers journaux personnalisés, mais il prend en charge la journalisation personnalisée.

Pour que les journaux Java soient disponibles dans AEM as a Cloud Service (via [Cloud Manager](#cloud-manager) ou [Interface de ligne de commande d’Adobe I/O](#aio)), les instructions de journal personnalisées doivent être écrites dans la variable `error.log`. Journaux écrits dans des journaux nommés personnalisés, tels que `example.log`, ne sera pas accessible à partir d’AEM as a Cloud Service.

Les journaux peuvent être écrits dans la variable `error.log` utilisation d’une propriété de configuration OSGi Sling LogManager dans le fichier `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` fichiers .

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Journaux du service Auteur et Publication AEM

Les services Auteur et Publication AEM fournissent AEM journaux de serveur d’exécution :

+ `aemerror` est le journal des erreurs Java (situé à l’adresse `/crx-quickstart/logs/error.log` sur le démarrage rapide local du SDK AEM). Voici les [niveaux de journal recommandés](#log-levels) pour les enregistreurs personnalisés par type d’environnement :
   + Développement: `DEBUG`
   + Évaluation: `WARN`
   + Production: `ERROR`
+ `aemaccess` répertorie les requêtes HTTP au service AEM avec des détails ;
+ `aemrequest` répertorie les requêtes HTTP effectuées au service AEM et leur réponse HTTP correspondante ;

## Journaux du Dispatcher de publication AEM

Seul AEM Publish Dispatcher fournit les journaux du serveur web Apache et de Dispatcher, car ces aspects n’existent que dans le niveau Publication AEM et non sur le niveau Auteur AEM.

+ `httpdaccess` répertorie les requêtes HTTP effectuées sur le serveur web Apache/Dispatcher du service AEM.
+ `httperror`  répertorie les messages de journal du serveur web Apache et aide au débogage des modules Apache pris en charge tels que `mod_rewrite`.
   + Développement: `DEBUG`
   + Évaluation: `WARN`
   + Production : `ERROR`
+ `aemdispatcher` répertorie les messages de journal des modules de Dispatcher, y compris le filtrage et la diffusion à partir des messages du cache.
   + Développement: `DEBUG`
   + Évaluation: `WARN`
   + Production : `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager permet le téléchargement des journaux par jour, via l’action Télécharger les journaux d’un environnement.

![Cloud Manager - Journaux de téléchargement](./assets/logs/download-logs.png)

Ces logs peuvent être téléchargés et inspectés à l’aide de n’importe quel outil d’analyse des logs.

## Adobe I/O de l’interface de ligne de commande avec le module externe Cloud Manager{#aio}

Adobe Cloud Manager prend en charge l’accès AEM journaux as a Cloud Service via le [Interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli) avec le [Module externe Cloud Manager pour l’interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Tout d’abord, [Configuration de l’Adobe I/O avec le module externe Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Assurez-vous que l’ID de programme et l’ID d’environnement appropriés ont été identifiés et utilisez [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) pour répertorier les options de journal utilisées pour [traîne](#aio-cli-tail-logs) ou [télécharger](#aio-cli-download-logs) journaux.

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

### Logs de fin{#aio-cli-tail-logs}

L’interface de ligne de commande d’Adobe I/O permet de consulter les journaux en temps réel à partir d’AEM as a Cloud Service à l’aide de la fonction [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) . Le suivi est utile pour observer l’activité du journal en temps réel lorsque des actions sont effectuées sur l’environnement as a Cloud Service AEM.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Autres outils de ligne de commande, tels que `grep` peut être utilisé de concert avec `tail-logs` pour isoler les instructions de journal présentant un intérêt, par exemple :

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... affiche uniquement les instructions de journal générées à partir de `com.example.MySlingModel` ou contiennent cette chaîne.

### Téléchargement des logs{#aio-cli-download-logs}

L’interface de ligne de commande d’Adobe I/O permet de télécharger les journaux depuis AEM as a Cloud Service à l’aide de la [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Cela produit le même résultat final que le téléchargement des journaux à partir de l’interface utilisateur web de Cloud Manager, la différence étant que `download-logs` La commande regroupe les journaux par jour, en fonction du nombre de jours demandé.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Compréhension des logs

Les journaux d’AEM as a Cloud Service contiennent plusieurs instructions de journal d’écriture de capsules. Comme plusieurs instances d’AEM écrivent dans le même fichier journal, il est important de comprendre comment analyser et réduire le bruit pendant le débogage. Pour expliquer : `aemerror` le fragment de code journal sera utilisé :

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

À l’aide des ID de capsule, le point de données après la date et l’heure, les journaux peuvent être regroupés par Capsule, ou AEM instance dans le service, ce qui facilite le suivi et la compréhension de l’exécution du code.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-22222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Niveaux de journal recommandés{#log-levels}

Les instructions générales de l’Adobe sur les niveaux de journal par environnement as a Cloud Service AEM sont les suivantes :

+ Développement local (AEM SDK) : `DEBUG`
+ Développement: `DEBUG`
+ Évaluation: `WARN`
+ Production : `ERROR`

La définition du niveau de journalisation le plus approprié pour chaque type d’environnement est avec AEM as a Cloud Service, les niveaux de journal sont conservés dans le code.

+ Les configurations de journal Java sont conservées dans les configurations OSGi.
+ Niveaux de journal du serveur web Apache et du Dispatcher dans le projet du Dispatcher

...et, par conséquent, nécessitent un déploiement pour changer.

### Variables spécifiques à l’environnement pour définir les niveaux de journal Java

Au lieu de définir des niveaux de journal Java statiques connus pour chaque environnement, vous pouvez utiliser AEM comme Cloud Service. [variables spécifiques à l’environnement](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) pour paramétrer les niveaux de journal, ce qui permet de modifier dynamiquement les valeurs au moyen de l’événement [Adobe I/O de l’interface de ligne de commande avec le module externe Cloud Manager](#aio-cli).

Cela nécessite la mise à jour des configurations OSGi de journalisation pour utiliser les espaces réservés de variable spécifiques à l’environnement. [Valeurs par défaut](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) pour les niveaux de journal doivent être définis conformément aux [Adobe des recommandations](#log-levels). Par exemple :

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Cette approche présente des inconvénients qui doivent être pris en compte :

+ [Un nombre limité de variables d’environnement est autorisé](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)et la création d’une variable pour gérer le niveau de journal en utilise une.
+ Les variables d’environnement ne peuvent être gérées que par programmation via [Interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) ou [API HTTP Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Les modifications apportées aux variables d’environnement doivent être réinitialisées manuellement par un outil pris en charge. L’oubli de réinitialiser un environnement à trafic élevé, tel que Production, à un niveau de journal moins détaillé peut inonder les journaux et affecter les performances AEM.

_Les variables spécifiques à l’environnement ne fonctionnent pas pour les configurations de journaux du serveur web Apache ou de Dispatcher, car elles ne sont pas configurées via la configuration OSGi._
