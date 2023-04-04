---
title: Développer des exportateurs de modèles Sling dans AEM
description: Cette présentation technique décrit la configuration de l’AEM à utiliser avec l’exportateur de modèles Sling, l’amélioration d’un modèle Sling existant à l’aide de la structure de l’exportateur pour le rendu au format JSON et la manière d’utiliser les options de l’exportateur et les annotations Jackson pour personnaliser davantage la sortie.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 3%

---

# Développer des exportateurs de modèles Sling

Cette présentation technique décrit la configuration de l’AEM à utiliser avec l’exportateur de modèles Sling, l’amélioration d’un modèle Sling existant à l’aide de la structure de l’exportateur pour le rendu au format JSON et la manière d’utiliser les options de l’exportateur et les annotations Jackson pour personnaliser davantage la sortie.

L’exportateur de modèle Sling a été introduit dans la version 1.3.0 des modèles Sling. Cette nouvelle fonctionnalité permet d’ajouter de nouvelles annotations aux modèles Sling qui définissent la manière dont le modèle peut être exporté sous la forme d’un objet Java différent, ou plus généralement sérialisé dans un autre format, tel que JSON.

Apache Sling fournit un exportateur JSON Jackson pour traiter le cas le plus courant d’exportation de modèles Sling en tant qu’objets JSON à des fins d’utilisation par des clients web programmatiques, tels que d’autres services web et des applications JavaScript.

## Configuration d’AEM pour l’exportateur de modèle Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] est une fonction de la fonction [!DNL Apache Sling] et ne sont pas directement liés au cycle de publication AEM produit. [!DNL Sling Model Exporter] est compatible avec AEM 6.3 et versions ultérieures.

## Le cas d’utilisation pour [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] est idéal pour tirer parti des modèles Sling qui contiennent déjà une logique métier prenant en charge les rendus de HTML via HTL (ou anciennement JSP) et exposer la même représentation métier que JSON pour une utilisation par des services Web programmatiques ou des applications JavaScript.

## Création d’un exportateur de modèle Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Activation [!DNL Exporter] prise en charge sur un [!DNL Sling Model] est aussi simple que d’ajouter la variable `@Exporter` annotation de la classe Java.

## Application des options de l’exportateur de modèle Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] prend en charge la transmission des options de l’exportateur par modèle à l’implémentation de l’exportateur pour déterminer comment la variable [!DNL Sling Model] est finalement exportée. Ces options s’appliquent généralement &quot;globalement&quot; à la manière dont la variable [!DNL Sling Model] est exporté, par rapport à chaque point de données qui peut être réalisé via des annotations intégrées décrites ci-dessous.

[!DNL Jackson Exporter] les options disponibles sont les suivantes :

* [Options des fonctions de mappage](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Options des fonctionnalités de sérialisation](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Application [!DNL Jackson] annotations

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Les mises en oeuvre d’exportateurs peuvent également prendre en charge les annotations qui peuvent être appliquées en ligne sur le [!DNL Sling Model] , qui peut fournir un niveau de contrôle plus fin de la manière dont les données sont exportées.

* [[!DNL Jackson Exporter] annotations](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Afficher le code {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Documents annexes {#supporting-materials}

* [[!DNL Jackson Mapper] Fonctionnalité Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Fonctionnalité Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documents](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
