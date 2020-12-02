---
title: Développer des exportateurs de modèles Sling en AEM
description: Cette marche technique vous guide tout au long des étapes nécessaires pour configurer l’AEM à utiliser avec Sling Model Exporter, améliorer un modèle Sling existant à l’aide de la structure Exporter pour le rendu en tant que JSON, et comment utiliser les options Exporter et les annotations Jackson pour personnaliser davantage la sortie.
version: 6.3, 6.4, 6.5
sub-product: fondation, content-services
feature: sling-models, sling-model-exporter
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---


# Développer les exportateurs de modèles Sling

Cette marche technique vous guide tout au long des étapes nécessaires pour configurer l’AEM à utiliser avec Sling Model Exporter, améliorer un modèle Sling existant à l’aide de la structure Exporter pour le rendu en tant que JSON, et comment utiliser les options Exporter et les annotations Jackson pour personnaliser davantage la sortie.

Sling Model Exporter a été introduit dans Sling Models v1.3.0. Cette nouvelle fonctionnalité permet d&#39;ajouter de nouvelles annotations aux modèles Sling qui définissent comment le modèle peut être exporté sous la forme d&#39;un objet Java différent, ou plus communément sérialisé dans un format différent tel que JSON.

Apache Sling fournit un exportateur JSON Jackson pour traiter le cas le plus courant d’exportation de modèles Sling en tant qu’objets JSON destinés à être consommés par des utilisateurs Web programmatiques, tels que d’autres services Web et applications JavaScript.

## Configuration de l’AEM pour l’exportateur de modèles Sling

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] est une fonctionnalité du  [!DNL Apache Sling] projet et n&#39;est pas directement liée au cycle de publication AEM produits. [!DNL Sling Model Exporter] est compatible avec AEM 6.3 et versions ultérieures.

## Le cas d’utilisation pour [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] est idéal pour tirer parti des modèles Sling qui contiennent déjà une logique métier prenant en charge les rendus HTML via HTL (ou précédemment JSP), et exposer la même représentation professionnelle que JSON pour une consommation par les services Web programmatiques ou les applications JavaScript.

## Création d’un exportateur de modèles Sling

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

L&#39;activation de la prise en charge de [!DNL Exporter] sur [!DNL Sling Model] est aussi simple que l&#39;ajout de l&#39;annotation `@Exporter` à la classe Java.

## Application des options de Sling Model Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] prend en charge la transmission des options d’exportateur par modèle à l’implémentation d’exportateur afin de déterminer comment le modèle  [!DNL Sling Model] est finalement exporté. Ces options s’appliquent généralement &quot;globalement&quot; à la manière dont [!DNL Sling Model] est exporté, par rapport à chaque point de données qui peut être effectué au moyen d’annotations intégrées décrites ci-dessous.

[!DNL Jackson Exporter] sont disponibles :

* [Options de fonction de mappage](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Options des fonctions de sérialisation](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Application des annotations [!DNL Jackson]

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Les implémentations d&#39;exportateurs peuvent également prendre en charge les annotations qui peuvent être appliquées en ligne sur la classe [!DNL Sling Model], ce qui peut fournir un niveau de contrôle plus précis sur la manière dont les données sont exportées.

* [[!DNL Jackson Exporter] annotations](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Vue du code {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Documents d&#39;appui {#supporting-materials}

* [[!DNL Jackson Mapper] Fonction Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Fonction Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Documents](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)
