---
title: Comprendre l'exportateur de modèles Sling en AEM
description: Apache Sling Models 1.3.0 présente Sling Model Exporter, un moyen élégant d’exporter ou de sérialiser des objets Sling Model dans des abstractions personnalisées. Cet article juxtapose le cas d’utilisation traditionnel de l’utilisation de modèles Sling pour renseigner des scripts HTL, en exploitant la structure Sling Model Exporter pour sérialiser un modèle Sling dans JSON.
version: 6.3, 6.4, 6.5
sub-product: fondation, content-services
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 1%

---


# Comprendre [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 présente [!DNL Sling Model Exporter], une manière élégante d’exporter ou de sérialiser [!DNL Sling Model] des objets dans des abstractions personnalisées. Cet article juxtapose le cas d’utilisation traditionnel de l’utilisation [!DNL Sling Models] pour renseigner des scripts HTML, en exploitant la [!DNL Sling Model Exporter] structure pour sérialiser un [!DNL Sling Model] fichier dans JSON.

## Flux de requête HTTP de modèle Sling traditionnel

Le cas d’utilisation traditionnel [!DNL Sling Models] consiste à fournir une abstraction commerciale pour une ressource ou une requête, qui fournit des scripts HTL (ou, précédemment, JSP) une interface pour accéder aux fonctions métier.

Les modèles courants se développent [!DNL Sling Models] qui représentent AEM Composants ou Pages, et utilisent les [!DNL Sling Model] objets pour alimenter les scripts HTML avec des données, avec un résultat final de HTML affiché dans le navigateur.

### Flux de requête HTTP de modèle Sling

![Flux de demande de modèle Sling](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Une demande est faite pour une ressource en AEM.

   Exemple: `HTTP GET /content/my-resource.html`

1. En fonction de la ressource de requête `sling:resourceType`, le script approprié est résolu.

1. Le script adapte la requête ou la ressource à la demande [!DNL Sling Model].

1. Le script utilise l’ [!DNL Sling Model] objet pour générer le rendu HTML.

1. Le code HTML généré par le script est renvoyé dans la réponse HTTP.

Ce modèle traditionnel fonctionne bien dans le contexte de la génération de code HTML, car il [!DNL Sling Model] peut être facilement exploité via HTL. La création de données plus structurées telles que JSON ou XML est une entreprise beaucoup plus fastidieuse car HTL ne se prête pas naturellement à la définition de ces formats.

## [!DNL Sling Model Exporter] Flux de requêtes HTTP

Apache [!DNL Sling Model Exporter] est fourni avec un Sling fourni par Jackson Exporter qui sérialise automatiquement un objet &quot;ordinaire&quot; [!DNL Sling Model] dans JSON. L’exportateur Jackson, bien que configurable, à son coeur inspecte l’ [!DNL Sling Model] objet et génère JSON en utilisant toutes les méthodes &quot;getter&quot; comme clés JSON, et les valeurs de retour getter comme valeurs JSON.

La sérialisation directe de [!DNL Sling Models] permet de traiter à la fois des requêtes Web normales avec leurs réponses HTML créées à l’aide du flux de [!DNL Sling Model] requêtes traditionnel (voir ci-dessus), mais aussi d’exposer des rendus JSON pouvant être utilisés par les services Web ou les applications JavaScript.

![Flux de requête HTTP Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Ce flux décrit le flux utilisant l’exportateur Jackson fourni pour produire la sortie JSON. L&#39;utilisation d&#39;exportateurs personnalisés suit le même flux mais avec leur format de sortie.*

1. GET HTTP La demande est faite pour une ressource en AEM avec le sélecteur et l’extension enregistrés auprès de l’ [!DNL Sling Model]Exportateur.

   Exemple: `HTTP GET /content/my-resource.model.json`

1. Sling résout le sélecteur `sling:resourceType`et l’extension de la ressource demandée en un servlet Sling Exporter généré de manière dynamique, qui est mappé à la [!DNL Sling Model] avec Exporter.
1. Le servlet Sling Exporter résolu appelle le [!DNL Sling Model Exporter] par rapport à l’ [!DNL Sling Model] objet adapté de la demande ou de la ressource (comme déterminé par les adaptables Sling Models).
1. L’exportateur sérialise les [!DNL Sling Model] données en fonction des options de l’exportateur et des annotations de modèle Sling propres à l’exportateur et renvoie le résultat au servlet Sling Exporter.
1. Le servlet Sling Exporter renvoie le rendu JSON du [!DNL Sling Model] dans la réponse HTTP.

>[!NOTE]
>
>Bien que le projet Apache Sling fournisse à Jackson Exporter le numéro de série [!DNL Sling Models] à JSON, la structure Exporter prend également en charge les Exportateurs personnalisés. Par exemple, un projet peut mettre en oeuvre un Exportateur personnalisé qui sérialise un fichier [!DNL Sling Model] dans XML.

>[!NOTE]
>
>Non seulement le [!DNL Sling Model Exporter] sérialisage *est-il possible* [!DNL Sling Models], mais il peut également les exporter en tant qu’objets Java. L’exportation vers d’autres objets Java ne joue pas de rôle dans le flux de requêtes HTTP et n’apparaît donc pas dans le diagramme ci-dessus.

## Documents de support

* [Documentation [!DNL Sling Model Exporter] d’ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
