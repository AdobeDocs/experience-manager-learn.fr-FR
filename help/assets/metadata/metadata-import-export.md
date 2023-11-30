---
title: Utiliser l’import et l’export de métadonnées dans AEM Assets
description: Découvrez comment utiliser les fonctions d’import et d’export des métadonnées d’Adobe Experience Manager Assets. Les fonctionnalités d’import et d’export permettent aux créateurs et créatrices de contenu de mettre à jour en masse les métadonnées des ressources existantes.
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 100%

---

# Utiliser l’import et l’export de métadonnées dans AEM Assets {#metadata-import-and-export}

Découvrez comment utiliser les fonctions d’import et d’export des métadonnées d’Adobe Experience Manager Assets. Les fonctionnalités d’import et d’export permettent aux créateurs et créatrices de contenu de mettre à jour en masse les métadonnées des ressources existantes.

## Export des métadonnées {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## Import de métadonnées {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> Lors de la préparation d’un fichier CSV à importer, il est plus facile de générer un fichier CSV avec la liste des ressources à l’aide de la fonction Export de métadonnées. Vous pouvez ensuite modifier le fichier CSV généré et l’importer à l’aide de la fonction Importer.

## Format de fichier CSV de métadonnées {#metadata-file-format}

### Première ligne

* La première ligne du fichier CSV définit le schéma de métadonnées.
* La première colonne est définie par défaut sur `assetPath`, qui contient le chemin d’accès JCR absolu d’une ressource.

* Les colonnes suivantes de la première ligne pointent vers d’autres propriétés de métadonnées d’une ressource.
   * Par exemple : `dc:title, dc:description, jcr:title`

* Format de propriété à valeur unique

   * `<metadata property name> {{<property type}}`
   * Si le type de propriété n’est pas spécifié, la valeur par défaut est Chaîne.
   * Par exemple : `dc:title {{String}}`

* Le nom de la propriété est sensible à la casse.
   * Correct : `dc:title {{String}}`
   * Incorrect : `Dc:Title {{String}}`

* Le type de propriété n’est pas sensible à la casse.
* Tous les [types de propriétés JCR](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) valides sont pris en charge.

* Format de propriété à plusieurs valeurs - `<metadata property name> {{<property type : MULTI }}`

### Deuxième ligne à N lignes

* La première colonne contient le chemin d’accès JCR absolu d’une ressource. Par exemple : /content/dam/asset1.jpg.
* La propriété de métadonnées d’une ressource peut avoir des valeurs manquantes dans le fichier CSV. Les propriétés de métadonnées manquantes pour cette ressource particulière ne sont pas mises à jour.
