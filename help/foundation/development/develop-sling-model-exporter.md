---
title: Développer des exporteurs Sling Model Exporter dans AEM
description: Cette présentation technique décrit la configuration d’AEM à utiliser avec Sling Model Exporter, l’amélioration d’un modèle Sling existant à l’aide de la structure de l’exporteur pour le rendu au format JSON et la manière d’utiliser les options de l’exporteur et les annotations Jackson pour personnaliser davantage la sortie.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 100%

---

# Développer des exporteurs Sling Model Exporter

Cette présentation technique décrit la configuration d’AEM à utiliser avec Sling Model Exporter, l’amélioration d’un modèle Sling existant à l’aide de la structure de l’exporteur pour le rendu au format JSON et la manière d’utiliser les options de l’exporteur et les annotations Jackson pour personnaliser davantage la sortie.

Sling Model Exporter a été introduit dans les modèles Sling v1.3.0. Cette nouvelle fonctionnalité permet d’ajouter de nouvelles annotations aux modèles Sling qui définissent la manière dont le modèle peut être exporté sous la forme d’un autre objet Java, ou plus généralement sérialisé dans un format différent tel que JSON.

Apache Sling fournit un exporteur JSON Jackson pour traiter le cas le plus courant d’export de modèles Sling en tant qu’objets JSON à des fins d’utilisation par des clients web programmatiques, par exemple d’autres services web et des applications JavaScript.

## Configurer AEM pour Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] est une fonction du projet [!DNL Apache Sling] qui n’est pas directement liée au cycle de publication du produit AEM. [!DNL Sling Model Exporter] est compatible avec AEM 6.3 et versions ultérieures.

## Le cas d’utilisation pour [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] est idéal pour tirer parti des modèles Sling qui contiennent déjà une logique métier prenant en charge les rendus HTML via HTL (anciennement JSP) et exposer la même représentation métier que JSON pour une utilisation par des services web programmatiques ou des applications JavaScript.

## Créer un exporteur Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

L’activation de la prise en charge de [!DNL Exporter] sur un [!DNL Sling Model] est aussi simple que d’ajouter l’annotation `@Exporter` à la classe Java.

## Appliquer les options de Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] prend en charge la transmission par modèle des options de l’exporteur à l’implémentation de l’exporteur pour déterminer comment le [!DNL Sling Model] est finalement exporté. Ces options s’appliquent généralement « globalement » à la manière dont le [!DNL Sling Model] est exporté, par rapport à l’export de chaque point de données qui peut être réalisée via des annotations intégrées décrites ci-dessous.

Les options [!DNL Jackson Exporter] incluent :

* [Options des fonctions de mappage](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Options des fonctionnalités de sérialisation](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Appliquer des annotations [!DNL Jackson]

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Les implémentations des exporteurs peuvent également prendre en charge les annotations qui peuvent être appliquées en ligne sur la classe [!DNL Sling Model], ce qui peut fournir un niveau de contrôle plus fin sur la manière dont les données sont exportées.

* [[!DNL Jackson Exporter] Annotations](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Afficher le code {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)

## Documents annexes {#supporting-materials}

* [[!DNL Jackson Mapper] Fonctionnalité Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Fonctionnalité Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documents](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
