---
title: Espaces de noms personnalisés
description: Découvrez comment définir et déployer des espaces de noms personnalisés dans AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 100%

---

# Espaces de noms personnalisés

Découvrez comment définir et déployer des [espaces de noms](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) personnalisés dans AEM as a Cloud Service.

Les espaces de noms personnalisés sont la partie facultative d’une propriété JCR précédant un `:`. AEM utilise plusieurs espaces de noms, tels que :

+ `jcr` pour les propriétés du système JCR.
+ `cq` pour les propriétés AEM (anciennement appelées Adobe CQ).
+ `dam` pour les propriétés AEM spécifiques aux ressources DAM.
+ `dc` pour les propriétés Dublin Core.

... et beaucoup d’autres.

Les espaces de noms peuvent être utilisés pour indiquer la portée et l’intention d’une propriété. La création d’un espace de noms personnalisés, souvent le nom de votre société, permet d’identifier clairement les nœuds ou les propriétés spécifiques à votre mise en œuvre AEM et de conserver des données spécifiques à votre entreprise.

Les espaces de noms personnalisés sont gérés dans les scripts d’[initialisation du référentiel Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html), et déployés vers AEM as a Cloud Service sous forme de configurations OSGi, puis ajoutés au projet `ui.config` de votre [projet AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr).

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Ressources

+ [Documentation sur l’initialisation du référentiel Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Code

Le code suivant est utilisé pour configurer un espace de noms `wknd`.

### Configuration OSGi de l’initialisateur de référentiel

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Cela permet d’utiliser les propriétés personnalisées à l’aide de l’espace de noms `wknd`, comme indiqué comme premier paramètre après l’instruction `register namespace` à utiliser dans AEM. Pour des définitions de script plus avancées, consultez les exemples de la [documentation sur l’initialisation du référentiel Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
