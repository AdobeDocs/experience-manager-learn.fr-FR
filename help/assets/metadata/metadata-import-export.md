---
title: Utilisation de l’importation et de l’exportation des métadonnées en AEM Assets
description: Découvrez comment utiliser les fonctions d’importation et d’exportation des métadonnées des ressources Adobe Experience Manager. Les fonctions d’importation et d’exportation permettent aux auteurs de contenu de mettre à jour en bloc les métadonnées des ressources existantes.
version: 6.3, 6.4, 6.5, cloud-service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 5%

---


# Utilisation de l’importation et de l’exportation des métadonnées en AEM Assets {#metadata-import-and-export}

Découvrez comment utiliser les fonctions d’importation et d’exportation des métadonnées des ressources Adobe Experience Manager. Les fonctions d’importation et d’exportation permettent aux auteurs de contenu de mettre à jour en bloc les métadonnées des ressources existantes.

## Exportation des métadonnées {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## Importation de métadonnées {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> Lors de la préparation de l’importation d’un fichier CSV, il est plus facile de générer un fichier CSV avec la liste des ressources à l’aide de la fonction d’exportation des métadonnées. Vous pouvez ensuite modifier le fichier CSV généré et l’importer à l’aide de la fonction d’importation.

## Format de fichier CSV de métadonnées {#metadata-file-format}

### Première rangée

* La première ligne du fichier CSV définit le schéma de métadonnées.
* La première colonne prend par défaut la valeur `assetPath`, qui contient le chemin d’accès JCR absolu pour une ressource.

* Les colonnes suivantes de la première ligne pointent vers d’autres propriétés de métadonnées d’un fichier.
   * Par exemple : `dc:title, dc:description, jcr:title`

* Format de propriété Valeur unique

   * `<metadata property name> {{<property type}}`
   * Si le type de propriété n’est pas spécifié, il prend par défaut la valeur String.
   * Par exemple : `dc:title {{String}}`

* Le nom de la propriété est sensible à la casse
   * Correct :`dc:title {{String}}`
   * Incorrect :`Dc:Title {{String}}`

* Le type de propriété n’est pas sensible à la casse
* Tous les types de propriétés [JCR valides](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) sont pris en charge

* Format de propriété de plusieurs valeurs - `<metadata property name> {{<property type : MULTI }}`

### Deuxième rangée à N lignes

* La première colonne contient le chemin d’accès JCR absolu pour une ressource. Par exemple : /content/dam/asset1.jpg
* Il se peut que la propriété de métadonnées d’un fichier comporte des valeurs manquantes dans le fichier CSV. Les propriétés de métadonnées manquantes pour cette ressource particulière ne sont pas mises à jour.
