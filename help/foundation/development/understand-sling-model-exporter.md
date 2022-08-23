---
title: Présentation de l’exportateur de modèle Sling dans AEM
description: Apache Sling Models 1.3.0 s’accompagne d’un outil d’exportation de modèles Sling, un moyen élégant d’exporter ou de sérialiser des objets de modèle Sling dans des abstractions personnalisées. Cet article juxtapose le cas d’utilisation traditionnel de l’utilisation de modèles Sling pour renseigner les scripts HTL, en utilisant la structure de l’exportateur de modèle Sling pour sérialiser un modèle Sling dans JSON.
version: 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# Comprendre [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduit [!DNL Sling Model Exporter], une méthode élégante d’exportation ou de sérialisation [!DNL Sling Model] dans des abstractions personnalisées. Cet article juxtapose le cas d’utilisation classique de la fonction [!DNL Sling Models] pour renseigner les scripts HTL, en exploitant la variable [!DNL Sling Model Exporter] structure pour sérialiser une [!DNL Sling Model] dans JSON.

## Flux de requête HTTP Sling Model traditionnel

Le cas d’utilisation classique pour [!DNL Sling Models] est de fournir une abstraction métier pour une ressource ou une requête, qui fournit des scripts HTL (ou, précédemment, des JSP) une interface pour l’accès aux fonctions métier.

Les modèles courants se développent [!DNL Sling Models] qui représentent AEM composants ou pages, et qui utilisent la variable [!DNL Sling Model] pour alimenter les scripts HTL avec des données, avec un résultat final de HTML affiché dans le navigateur.

### Flux de requête HTTP de modèle Sling

![Flux de requête de modèle Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La demande est faite pour une ressource dans AEM.

   Exemple : `HTTP GET /content/my-resource.html`

1. Basé sur le de la ressource de requête `sling:resourceType`, le script approprié est résolu.

1. Le script adapte la requête ou la ressource à la demande souhaitée. [!DNL Sling Model].

1. Le script utilise la variable [!DNL Sling Model] pour générer le rendu de HTML.

1. Le HTML généré par le script est renvoyé dans la réponse HTTP.

Ce modèle traditionnel fonctionne bien dans le contexte de la génération de HTML comme [!DNL Sling Model] peut être facilement exploité via HTL. La création de données plus structurées telles que JSON ou XML est une tâche beaucoup plus fastidieuse, car HTL ne se prête pas naturellement à la définition de ces formats.

## [!DNL Sling Model Exporter] Flux de requête HTTP

Apache [!DNL Sling Model Exporter] est fourni avec un exportateur Jackson fourni par Sling qui sérialise automatiquement un &quot;ordinaire&quot; [!DNL Sling Model] dans JSON. L&#39;exportateur Jackson, bien que configurable, inspecte en profondeur la variable [!DNL Sling Model] et génère JSON en utilisant n’importe quelle méthode &quot;getter&quot; comme clés JSON, et les valeurs getter comme valeurs JSON.

La sérialisation directe de [!DNL Sling Models] leur permet de traiter les deux requêtes web normales avec leurs réponses de HTML créées à l’aide de la méthode [!DNL Sling Model] flux de requêtes (voir ci-dessus), mais exposent également les rendus JSON pouvant être utilisés par les services web ou les applications JavaScript.

![Flux de requête HTTP Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Ce flux décrit le flux à l’aide de l’exportateur Jackson fourni pour produire la sortie JSON. L&#39;utilisation d&#39;exportateurs personnalisés suit le même flux mais avec leur format de sortie.*

1. Une requête de GET HTTP est effectuée pour une ressource dans AEM avec le sélecteur et l’extension enregistrée avec l’ [!DNL Sling Model]Exportateur de .

   Exemple : `HTTP GET /content/my-resource.model.json`

1. Sling résout le de la ressource demandée `sling:resourceType`, le sélecteur et l’extension à un servlet Sling Exporter généré dynamiquement, qui est mappé sur le [!DNL Sling Model] avec l’exportateur.
1. Le servlet Sling Exporter résolu appelle la variable [!DNL Sling Model Exporter] par rapport à [!DNL Sling Model] adapté de la requête ou de la ressource (comme déterminé par les modèles Sling adaptables).
1. L’exportateur sérialise la variable [!DNL Sling Model] en fonction des options de l’exportateur et des annotations de modèle Sling spécifiques à l’exportateur et renvoie le résultat au servlet Sling Exporter.
1. Le servlet d’exportateur Sling renvoie le rendu JSON de la variable [!DNL Sling Model] dans la réponse HTTP.

>[!NOTE]
>
>Alors que le projet Apache Sling fournit l’exportateur Jackson qui sérialise [!DNL Sling Models] sur JSON, le framework Exporter prend également en charge les exportateurs personnalisés. Par exemple, un projet peut mettre en oeuvre un exportateur personnalisé qui sérialise un [!DNL Sling Model] dans XML.

>[!NOTE]
>
>Non seulement le [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models], il peut également les exporter en tant qu’objets Java. L’export vers d’autres objets Java ne joue pas de rôle dans le flux de requête HTTP et n’apparaît donc pas dans le diagramme ci-dessus.

## Documents annexes

* [Apache [!DNL Sling Model Exporter] Documentation du framework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
