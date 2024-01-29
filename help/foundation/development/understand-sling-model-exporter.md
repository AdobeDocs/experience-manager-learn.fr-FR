---
title: Comprendre Sling Model Exporter dans AEM
description: Apache Sling Models 1.3.0 lance Sling Model Exporter, un moyen efficace d’exporter ou de sérialiser des objets de modèle Sling Model dans des abstractions personnalisées. Cet article compare le cas d’utilisation traditionnel des modèles Sling pour alimenter les scripts HTL, avec le framework Sling Model Exporter pour sérialiser un modèle Sling Model en JSON.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 170
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '568'
ht-degree: 100%

---

# Comprendre [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 lance [!DNL Sling Model Exporter], un moyen efficace d’exporter ou de sérialiser des objets de modèle [!DNL Sling Model] dans des abstractions personnalisées. Cet article compare le cas d’utilisation traditionnel des modèles [!DNL Sling Models] pour alimenter les scripts HTL, avec le framework [!DNL Sling Model Exporter] pour sérialiser un modèle [!DNL Sling Model] en JSON.

## Flux de requête HTTP Sling Model traditionnel

Le cas d’utilisation traditionnel de [!DNL Sling Models] consiste à fournir une abstraction commerciale pour une ressource ou une demande, qui offre aux scripts HTL (ou, précédemment, aux JSP) une interface pour accéder aux fonctions commerciales.

Les modèles courants développent des [!DNL Sling Models] qui représentent les composants ou pages AEM, et qui utilisent les objets [!DNL Sling Model] pour alimenter les scripts HTL avec des données et un résultat final de HTML affiché dans le navigateur.

### Flux de requête HTTP Sling Model

![Flux de requête Sling Model.](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. La requête [!DNL HTTP GET] est faite pour une ressource dans AEM.

   Exemple : `HTTP GET /content/my-resource.html`

1. Le script approprié se résout en fonction du `sling:resourceType` de la ressource demandée.

1. Le script adapte la demande ou la ressource au [!DNL Sling Model] souhaité.

1. Le script utilise l’objet [!DNL Sling Model] pour générer le rendu de HTML.

1. Le HTML généré par le script est renvoyé dans la réponse HTTP.

Ce modèle traditionnel fonctionne bien dans le contexte de la génération de HTML puisque le [!DNL Sling Model] peut être facilement exploité via HTL. La création de données plus structurées telles que JSON ou XML est une tâche beaucoup plus fastidieuse, car HTL ne se prête pas à la définition de ces formats.

## Flux de requête HTTP [!DNL Sling Model Exporter]

Le [!DNL Sling Model Exporter] d’Apache est fourni avec un exporteur Jackson fourni par Sling qui sérialise automatiquement un objet [!DNL Sling Model] « ordinaire » en JSON. L’exporteur Jackson, bien que configurable, inspecte en détail l’objet [!DNL Sling Model] et génère le JSON en utilisant n’importe quelle méthode « getter » en tant que clés JSON et les valeurs getter sont renvoyées en tant que valeurs JSON.

La sérialisation directe de [!DNL Sling Models] permet de répondre à la fois aux demandes web normales avec leurs réponses HTML créées à l’aide du flux de requête traditionnel [!DNL Sling Model] (voir ci-dessus), mais aussi d’exposer des rendus JSON qui peuvent être consommés par des services web ou des applications JavaScript.

![Flux de requête HTTP Sling Model Exporter.](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Ce flux décrit le flux à l’aide de l’exporteur Jackson fourni pour produire la sortie JSON. L’utilisation d’exporteurs personnalisés suit le même flux, mais avec leur format de sortie.*

1. Une requête HTTP GET est effectuée pour une ressource dans AEM avec le sélecteur et l’extension enregistrés avec le [!DNL Sling Model] Exporter.

   Exemple : `HTTP GET /content/my-resource.model.json`

1. Sling résout le `sling:resourceType`, le sélecteur et l’extension de la ressource demandée vers un servlet Sling Exporter généré dynamiquement, qui est mappé sur le [!DNL Sling Model] avec l’Exporter.
1. Le servlet Sling Exporter résolu appelle le [!DNL Sling Model Exporter] par rapport à l’objet [!DNL Sling Model] adapté de la requête ou de la ressource (comme défini par les adaptables des Sling Models).
1. L’exporteur sérialise le [!DNL Sling Model] en fonction ses options et des annotations de Sling Model qui lui sont spécifiques et renvoie le résultat au servlet de Sling Exporter.
1. Le servlet de Sling Exporter renvoie le rendu JSON du [!DNL Sling Model] dans la réponse HTTP.

>[!NOTE]
>
>Bien que le projet Apache Sling fournisse l’exporteur Jackson qui sérialise [!DNL Sling Models] en JSON, le framework d’exporteur prend également en charge les exporteurs personnalisés. Par exemple, un projet peut mettre en œuvre un exporteur personnalisé qui sérialise un [!DNL Sling Model] en XML.

>[!NOTE]
>
>Non seulement l’[!DNL Sling Model Exporter] *sérialise* les [!DNL Sling Models], mais il peut également les exporter en tant qu’objets Java. L’export vers d’autres objets Java ne contribue pas au flux de requête HTTP et n’apparaît donc pas dans le diagramme ci-dessus.

## Documents annexes

* [Documentation framework Apache [!DNL Sling Model Exporter] ](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
