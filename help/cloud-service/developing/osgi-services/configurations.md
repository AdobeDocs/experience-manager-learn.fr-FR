---
title: Propriétés de configuration OSGi
description: Découvrez les principes de base des propriétés de configuration OSGi et comment les utiliser dans les services OSGi.
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8268
thumbnail: 335729.jpeg
exl-id: 096b0a95-7039-4570-b567-ba316bfc8709
duration: 755
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '52'
ht-degree: 100%

---

# Propriétés de configuration OSGi

Découvrez l’approche de bas niveau de l’utilisation de paires clé/valeur de configuration OSGi pour définir et exposer les données de configuration OSGi aux services OSGi.

>[!VIDEO](https://video.tv.adobe.com/v/3417822?quality=12&learn=on&captions=fre_fr)

## Ressources

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@Activate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)

## Code

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Map;
import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Deactivate;
import org.osgi.util.converter.Converters;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class },
    property = {
        "activities=Hiking",
        "activities=Jogging",
        "activities=Walking",
        "random.seed:Integer=10"
    }
)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    private String[] activities;

    private Random random;

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate(Map<String, Object> properties) {

        this.activities = Converters.standardConverter()
                .convert(properties.get("activities"))
                .defaultValue(new String[] { 
                    "Default Activity 1",
                    "Default Activity 2"
                })
                .to(String[].class);

        final Integer randomSeed = Converters.standardConverter()
                .convert(properties.get("random.seed"))
                .defaultValue(25)
                .to(Integer.class);   
        
        this.random = new Random(randomSeed);

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```

### com.adobe.aem.wknd.examples.core.adventures.impl.ActivitiesImpl.cfg.json

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/com.adobe.aem.wknd.examples.core.adventures.impl.ActivitiesImpl.cfg.json`

```javascript
{
    "activities": [ "Skateboarding", "Surfing", "Skiing" ],
    "random.seed:Integer": 32
}
```
