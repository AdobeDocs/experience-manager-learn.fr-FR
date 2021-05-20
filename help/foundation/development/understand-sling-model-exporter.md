---
title: Présentation de l’exportateur de modèle Sling dans AEM
description: Apache Sling Models 1.3.0 s’accompagne d’un outil d’exportation de modèles Sling, un moyen élégant d’exporter ou de sérialiser des objets de modèle Sling dans des abstractions personnalisées. Cet article juxtapose le cas d’utilisation traditionnel de l’utilisation de modèles Sling pour renseigner les scripts HTL, en utilisant la structure de l’exportateur de modèle Sling pour sérialiser un modèle Sling dans JSON.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: les API ;
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---


# Comprendre [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 introduit [!DNL Sling Model Exporter], un moyen élégant d’exporter ou de sérialiser des objets [!DNL Sling Model] dans des abstractions personnalisées. Cet article juxtapose le cas d’utilisation traditionnel de l’utilisation de [!DNL Sling Models] pour renseigner les scripts HTL, en utilisant la structure [!DNL Sling Model Exporter] pour sérialiser une [!DNL Sling Model] dans JSON.

## Flux de requête HTTP Sling Model traditionnel

Le cas d’utilisation traditionnel de [!DNL Sling Models] consiste à fournir une abstraction commerciale pour une ressource ou une requête, qui fournit des scripts HTL (ou, précédemment, des JSP) une interface pour l’accès aux fonctions métier.

Les modèles courants sont en train de développer [!DNL Sling Models] qui représentent AEM composants ou pages, et d’utiliser les objets [!DNL Sling Model] pour alimenter les scripts HTL avec des données, avec un résultat final de code HTML affiché dans le navigateur.

### Flux de requête HTTP de modèle Sling

![Flux de requête de modèle Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] La demande est faite pour une ressource dans AEM.

   Exemple: `HTTP GET /content/my-resource.html`

1. En fonction de `sling:resourceType` de la ressource de requête, le script approprié est résolu.

1. Le script adapte la requête ou la ressource à la [!DNL Sling Model] souhaitée.

1. Le script utilise l’objet [!DNL Sling Model] pour générer le rendu HTML.

1. Le code HTML généré par le script est renvoyé dans la réponse HTTP.

Ce modèle traditionnel fonctionne bien dans le contexte de la génération de code HTML, car la fonction [!DNL Sling Model] peut être facilement exploitée via HTL. La création de données plus structurées telles que JSON ou XML est une tâche beaucoup plus fastidieuse, car HTL ne se prête pas naturellement à la définition de ces formats.

## [!DNL Sling Model Exporter] Flux de requête HTTP

Apache [!DNL Sling Model Exporter] est fourni avec un exportateur Jackson fourni par Sling qui sérialise automatiquement un objet [!DNL Sling Model] &quot;ordinaire&quot; dans JSON. L’exportateur Jackson, bien que configurable, à son coeur inspecte l’objet [!DNL Sling Model] et génère JSON en utilisant n’importe quelle méthode &quot;getter&quot; comme clés JSON, et les valeurs de retour getter comme valeurs JSON.

La sérialisation directe de [!DNL Sling Models] leur permet de traiter à la fois des requêtes Web normales avec leurs réponses HTML créées à l’aide du flux de requêtes [!DNL Sling Model] traditionnel (voir ci-dessus), mais aussi d’exposer des rendus JSON pouvant être utilisés par des services Web ou des applications JavaScript.

![Flux de requête HTTP Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Ce flux décrit le flux à l’aide de l’exportateur Jackson fourni pour produire la sortie JSON. L&#39;utilisation d&#39;exportateurs personnalisés suit le même flux mais avec leur format de sortie.*

1. Une requête de GET HTTP est faite pour une ressource en AEM avec le sélecteur et l’extension enregistrés auprès de l’exportateur [!DNL Sling Model].

   Exemple: `HTTP GET /content/my-resource.model.json`

1. Sling résout le `sling:resourceType`, le sélecteur et l’extension de la ressource demandée en un servlet d’exportateur Sling généré dynamiquement, qui est mappé sur la [!DNL Sling Model] avec l’exportateur.
1. Le servlet Sling Exporter résolu appelle la balise [!DNL Sling Model Exporter] par rapport à l’objet [!DNL Sling Model] adapté de la requête ou de la ressource (tel que déterminé par les modèles Sling adaptables).
1. L’exportateur sérialise le [!DNL Sling Model] en fonction des annotations de modèle Sling spécifiques à l’exportateur et renvoie le résultat au servlet d’exportateur Sling.
1. Le servlet d’exportateur Sling renvoie le rendu JSON de [!DNL Sling Model] dans la réponse HTTP.

>[!NOTE]
>
>Bien que le projet Apache Sling fournisse l’exportateur Jackson qui sérialise [!DNL Sling Models] vers JSON, la structure d’exportateur prend également en charge les exportateurs personnalisés. Par exemple, un projet peut implémenter un exportateur personnalisé qui sérialise [!DNL Sling Model] en XML.

>[!NOTE]
>
>[!DNL Sling Model Exporter] *serialize* [!DNL Sling Models], il peut également les exporter en tant qu’objets Java. L’export vers d’autres objets Java ne joue pas de rôle dans le flux de requête HTTP et n’apparaît donc pas dans le diagramme ci-dessus.

## Documents annexes

* [ [!DNL Sling Model Exporter] Documentation ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
