---
title: Espaces de noms personnalisés
description: Découvrez comment définir et déployer des espaces de noms personnalisés pour AEM as a Cloud Service.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
kt: 11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 6%

---

# Espaces de noms personnalisés

Découvrez comment définir et déployer des [espaces de noms](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) à AEM as a Cloud Service.

Les espaces de noms personnalisés sont la partie facultative d’une propriété JCR précédant un `:`. AEM utilise plusieurs espaces de noms, tels que :

+ `jcr` pour les propriétés du système JCR
+ `cq` pour les propriétés d’AEM (anciennement appelées Adobe CQ)
+ `dam` pour les propriétés AEM spécifiques aux ressources DAM
+ `dc` pour les propriétés Dublin Core

... et beaucoup d&#39;autres.

Les espaces de noms peuvent être utilisés pour indiquer la portée et l’intention d’une propriété. La création d’un espace de noms personnalisé, souvent le nom de votre société, permet d’identifier clairement les noeuds ou les propriétés spécifiques à votre mise en oeuvre AEM et de contenir des données spécifiques à votre entreprise.

Les espaces de noms personnalisés sont gérés dans [Initialisation du référentiel Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) des scripts et des déploiements pour AEM des configurations OSGi as a Cloud Service, et les ajouter à vos [AEM projet](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=fr) `ui.config` projet.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Ressources

+ [Documentation sur l’initialisation du référentiel Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Code

Le code suivant est utilisé pour configurer une `wknd` espace de noms.

### Configuration OSGi de RepositoryInitializer

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Cela permet d’utiliser les propriétés personnalisées à l’aide de la propriété `wknd` , comme indiqué comme premier paramètre après l’événement `register namespace` à utiliser dans AEM. Pour des définitions de script plus avancées, consultez les exemples de la section [Documentation sur l’initialisation du référentiel Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
