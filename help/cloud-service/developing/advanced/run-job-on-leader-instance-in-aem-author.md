---
title: Exécution d’une tâche sur l’instance principale dans AEM as a Cloud Service
description: Découvrez comment exécuter une tâche sur l’instance principale dans AEM as a Cloud Service.
version: Cloud Service
topic: Development
feature: OSGI, Cloud Manager
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Article
duration: 0
last-substantial-update: 2024-10-23T00:00:00Z
jira: KT-16399
thumbnail: KT-16399.jpeg
source-git-commit: 7dca86137d476418c39af62c3c7fa612635c0583
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---


# Exécution d’une tâche sur l’instance principale dans AEM as a Cloud Service

Découvrez comment exécuter une tâche sur l’instance principale dans le service de création d’AEM dans AEM as a Cloud Service et comment la configurer pour qu’elle s’exécute une seule fois.

Les tâches Sling sont des tâches asynchrones qui fonctionnent en arrière-plan, conçues pour gérer les événements système ou déclenchés par l’utilisateur. Par défaut, ces tâches sont réparties uniformément entre toutes les instances (capsules) de la grappe.

Pour plus d’informations, voir [Gestion des tâches et des événements Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html).

## Création et traitement de tâches

À des fins de démonstration, créons une tâche _simple qui demande au processeur de tâches de consigner un message_.

### Création d’une tâche

Utilisez le code ci-dessous pour _créer_ une tâche Apache Sling :

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import java.util.HashMap;
import java.util.Map;

import org.apache.sling.event.jobs.JobManager;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class SimpleJobCreaterImpl {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobCreaterImpl.class);

    // Define the topic on which the job will be created
    protected static final String TOPIC = "wknd/simple/job/topic";

    // Inject a JobManager
    @Reference
    private JobManager jobManager;

    @Activate
    protected final void activate() throws Exception {
        log.info("SimpleJobCreater activated successfully");
        createJob();
        log.info("SimpleJobCreater created a job");
    }

    private void createJob() {
        // Create a job and add it on the above defined topic
        Map<String, Object> jobProperties = new HashMap<>();
        jobProperties.put("action", "log");
        jobProperties.put("message", "Job metadata is: Created in activate method");
        jobManager.addJob(TOPIC, jobProperties);
    }
}
```

Les points clés à noter dans le code ci-dessus sont les suivants :

- La charge utile de tâche possède deux propriétés : `action` et `message`.
- À l’aide de la méthode `addJob(...)` de [JobManager](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/apache/sling/event/jobs/JobManager.html), la tâche est ajoutée à la rubrique `wknd/simple/job/topic`.

### Traitement d’une tâche

Utilisez le code ci-dessous pour _process_ de la tâche Apache Sling ci-dessus :

```java
package com.adobe.aem.guides.wknd.core.sling.jobs.impl;

import org.apache.sling.event.jobs.Job;
import org.apache.sling.event.jobs.consumer.JobConsumer;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = JobConsumer.class, property = {
        JobConsumer.PROPERTY_TOPICS + "=" + SimpleJobCreaterImpl.TOPIC
}, immediate = true)
public class SimpleJobConsumerImpl implements JobConsumer {

    private static final Logger log = LoggerFactory.getLogger(SimpleJobConsumerImpl.class);

    @Override
    public JobResult process(Job job) {
        // Get the action and message properties
        String action = job.getProperty("action", String.class);
        String message = job.getProperty("message", String.class);

        // Log the message
        if ("log".equals(action)) {
            log.info("Processing WKND Job, and {}", message);
        }

        // Return a successful result
        return JobResult.OK;
    }

}
```

Les points clés à noter dans le code ci-dessus sont les suivants :

- La classe `SimpleJobConsumerImpl` met en oeuvre l’interface `JobConsumer`.
- Il s’agit d’un service enregistré pour consommer des tâches de la rubrique `wknd/simple/job/topic`.
- La méthode `process(...)` traite la tâche en enregistrant la propriété `message` de la charge utile de tâche.

### Traitement des tâches par défaut

Lorsque vous déployez le code ci-dessus dans un environnement AEM as a Cloud Service et que vous l’exécutez sur le service d’auteur d’AEM, qui fonctionne comme une grappe avec plusieurs JVM d’auteur d’AEM, la tâche s’exécute une fois sur chaque instance d’auteur d’AEM (pod), ce qui signifie que le nombre de tâches créées correspondra au nombre de capsules. Le nombre de capsules sera toujours supérieur à un (pour les environnements non-RDE), mais fluctuera en fonction de la gestion des ressources internes d’AEM as a Cloud Service.

La tâche est exécutée sur chaque instance d’auteur AEM (pod), car `wknd/simple/job/topic` est associé à AEM file d’attente principale d’qui distribue les tâches sur toutes les instances disponibles.

Cela est souvent problématique si la tâche est responsable de la modification de l’état, comme la création ou la mise à jour de ressources ou de services externes.

Si vous souhaitez que la tâche ne s’exécute qu’une seule fois sur le service d’auteur AEM, ajoutez la [configuration de la file d’attente de travaux](#how-to-run-a-job-on-the-leader-instance) décrite ci-dessous.

Vous pouvez le vérifier en consultant les journaux du service d’auteur AEM dans [Cloud Manager](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/logs#cloud-manager).

![Traitement de la tâche par toutes les instances](./assets/run-job-once/job-processed-by-all-instances.png)


Vous devriez voir :

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-nxxcx] *INFO* [sling-oak-observation-15] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method

<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-68775db964-r4zk7] *INFO* [sling-oak-observation-11] org.apache.sling.event.impl.jobs.queues.JobQueueImpl.<main queue> Starting job queue <main queue>
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```

Il existe deux entrées de journal, une pour chaque instance d’auteur AEM (`68775db964-nxxcx` et `68775db964-r4zk7`), indiquant que chaque instance (capsule) a traité la tâche.

## Exécution d’une tâche sur l’instance principale

Pour exécuter une tâche _une seule fois_ sur le service d’auteur AEM, créez une nouvelle file d’attente de tâche Sling de type **Ordered** et associez votre rubrique de tâche (`wknd/simple/job/topic`) à cette file d’attente. Avec cette configuration, seule l’instance d’auteur (pod) de l’AEM principale pourra traiter la tâche.

Dans le module `ui.config` de votre projet AEM, créez un fichier de configuration OSGi (`org.apache.sling.event.jobs.QueueConfiguration~wknd.cfg.json`) et stockez-le dans le dossier `ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author`.

```json
{
    "queue.name":"WKND Queue - ORDERED",
    "queue.topics":[
      "wknd/simple/job/topic"
    ],
    "queue.type":"ORDERED",
    "queue.retries":1,
    "queue.maxparallel":1.0
  }
```

Les points clés à noter dans la configuration ci-dessus sont les suivants :

- La rubrique de la file d’attente est définie sur `wknd/simple/job/topic`.
- Le type de file d’attente est défini sur `ORDERED`.
- Le nombre maximal de tâches parallèles est défini sur `1`.

Une fois la configuration ci-dessus déployée, la tâche est traitée exclusivement par l’instance principale, en s’assurant qu’elle ne s’exécute qu’une seule fois sur l’ensemble du service d’auteur AEM.

```
<DD.MM.YYYY HH:mm:ss.SSS> [cm-pxxxx-exxxx-aem-author-7475cf85df-qdbq5] *INFO* [FelixLogListener] Events.Service.org.apache.sling.event Service [QueueMBean for queue WKND Queue - ORDERED,7755, [org.apache.sling.event.jobs.jmx.StatisticsMBean]] ServiceEvent REGISTERED
<DD.MM.YYYY HH:mm:ss.SSS> INFO [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
<DD.MM.YYYY HH:mm:ss.SSS> [com.adobe.aem.guides.wknd.core.sling.jobs.impl.SimpleJobConsumerImpl] Processing WKND Job, and Job metadata is: Created in activate method
```
