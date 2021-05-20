---
title: Développer des exportateurs de modèles Sling dans AEM
description: Cette présentation technique décrit la configuration de l’AEM à utiliser avec l’exportateur de modèles Sling, l’amélioration d’un modèle Sling existant à l’aide de la structure de l’exportateur pour le rendu au format JSON et la manière d’utiliser les options de l’exportateur et les annotations Jackson pour personnaliser davantage la sortie.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: les API ;
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 3%

---


# Développer des exportateurs de modèles Sling

Cette présentation technique décrit la configuration de l’AEM à utiliser avec l’exportateur de modèles Sling, l’amélioration d’un modèle Sling existant à l’aide de la structure de l’exportateur pour le rendu au format JSON et la manière d’utiliser les options de l’exportateur et les annotations Jackson pour personnaliser davantage la sortie.

L’exportateur de modèle Sling a été introduit dans la version 1.3.0 des modèles Sling. Cette nouvelle fonctionnalité permet d’ajouter de nouvelles annotations aux modèles Sling qui définissent la manière dont le modèle peut être exporté sous la forme d’un objet Java différent, ou plus généralement sérialisé dans un autre format, tel que JSON.

Apache Sling fournit un exportateur JSON Jackson pour traiter le cas le plus courant d’exportation de modèles Sling en tant qu’objets JSON à des fins d’utilisation par des clients web programmatiques, tels que d’autres services web et des applications JavaScript.

## Configuration d’AEM pour l’exportateur de modèle Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] est une fonctionnalité du  [!DNL Apache Sling] projet qui n’est pas directement liée au cycle de publication AEM produit. [!DNL Sling Model Exporter] est compatible avec AEM 6.3 et versions ultérieures.

## Le cas d’utilisation de [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] est idéal pour tirer parti des modèles Sling qui contiennent déjà une logique métier prenant en charge les rendus HTML via HTL (ou anciennement JSP) et exposer la même représentation professionnelle que JSON pour une utilisation par des services Web programmatiques ou des applications JavaScript.

## Création d’un exportateur de modèle Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

L’activation de la prise en charge de [!DNL Exporter] sur un [!DNL Sling Model] est aussi simple que l’ajout de l’annotation `@Exporter` à la classe Java.

## Application des options de l’exportateur de modèle Sling

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] prend en charge la transmission des options d’exportateur par modèle à l’implémentation de l’exportateur afin de déterminer comment le  [!DNL Sling Model] est finalement exporté. Ces options s’appliquent généralement &quot;globalement&quot; à la manière dont la balise [!DNL Sling Model] est exportée, par rapport à chaque point de données qui peut être réalisé via des annotations intégrées décrites ci-dessous.

[!DNL Jackson Exporter] les options disponibles sont les suivantes :

* [Options des fonctions de mappage](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Options des fonctionnalités de sérialisation](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Application des annotations [!DNL Jackson]

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Les mises en oeuvre d’exportateurs peuvent également prendre en charge les annotations qui peuvent être appliquées en ligne sur la classe [!DNL Sling Model], ce qui peut fournir un niveau de contrôle plus fin sur la manière dont les données sont exportées.

* [[!DNL Jackson Exporter] annotations](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Afficher le code {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Documents complémentaires {#supporting-materials}

* [[!DNL Jackson Mapper] Fonctionnalité Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Fonctionnalité Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documents](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
