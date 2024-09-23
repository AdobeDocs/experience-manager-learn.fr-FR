---
title: Concepts de base du développement des services OSGi
description: Découvrez les concepts de base du développement d’un service OSGi.
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 505
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '122'
ht-degree: 100%

---

# Services OSGi

Découvrez les concepts de base du développement de services OSGi, notamment les éléments suivants :

+ Comment convertir un POJO Java en service OSGi.
+ Comment lier un service OSGi à une interface Java.

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## Ressources

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@ProviderType JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## Code

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```

L’ajout d’un `package-info.java` est requis pour s’assurer que d’autres lots OSGi dans AEM peuvent résoudre l’interface de service OSGi (ou toute classe Java). Si le `package-info.java` est manquant, le package Java et ses interfaces ou classes Java ne sont pas exportés. Les autres lots OSGi qui tentent d’importer ces interfaces ou classes Java à partir de ce package Java afficheront le message d’erreur __Impossible de résoudre__ dans la console du lot OSGi d’AEM.
