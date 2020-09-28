---
title: Utilisation de l’importation et de l’exportation des métadonnées en AEM Assets
seo-title: Utilisation de l’importation et de l’exportation des métadonnées en AEM Assets
description: Les fonctions d’importation et d’exportation des métadonnées AEM Assets permettent aux auteurs de contenu de déplacer facilement les métadonnées de fichiers vers AEM et hors de l’et d’exploiter la puissance de Microsoft Excel pour manipuler les métadonnées à l’échelle, ce qui facilite la mise à jour en masse des métadonnées pour les ressources existantes dans AEM.
seo-description: Les fonctions d’importation et d’exportation des métadonnées AEM Assets permettent aux auteurs de contenu de déplacer facilement les métadonnées de fichiers vers AEM et hors de l’et d’exploiter la puissance de Microsoft Excel pour manipuler les métadonnées à l’échelle, ce qui facilite la mise à jour en masse des métadonnées pour les ressources existantes dans AEM.
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 3%

---


# Utilisation de l’importation et de l’exportation des métadonnées en AEM Assets{#using-metadata-import-and-export-in-aem-assets}

Les fonctions d’importation et d’exportation des métadonnées AEM Assets permettent aux auteurs de contenu de déplacer facilement les métadonnées de fichiers vers AEM et hors de l’et d’exploiter la puissance de Microsoft Excel pour manipuler les métadonnées à l’échelle, ce qui facilite la mise à jour en masse des métadonnées pour les ressources existantes dans AEM.

## Exportation des métadonnées {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## Importation de métadonnées {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

Télécharger le dossier sports [WeRetail](assets/we-retail-sports.zip)

Téléchargement du package de métadonnées [de fichier](assets/we-retail-sports-asset-metadata.zip)

## Format du fichier de métadonnées {#metadata-file-format}

### Format de fichier CSV

#### Première rangée

* La première ligne du fichier CSV définit le schéma de métadonnées.
* La première colonne est définie par défaut sur `assetPath`, qui contient le chemin d’accès JCR absolu pour une ressource.

* Les colonnes suivantes de la première ligne pointent vers d’autres propriétés de métadonnées d’un fichier.

   * Par exemple : `dc:title, dc:description, jcr:title`

* Format de propriété Valeur unique

   * `<metadata property name> {{<property type}}`
   * Si le type de propriété n&#39;est pas spécifié, il prend par défaut la valeur String.
   * Par exemple : `dc:title {{String}}`

* Le nom de la propriété est sensible à la casse
   * Correct :`dc:title {{String}}`
   * Incorrect :`Dc:Ttle {{String}}`

* Le type de propriété n’est pas sensible à la casse
* Tous les types [de propriétés](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) JCR valides sont pris en charge

* Format de propriété à plusieurs valeurs - `<metadata property name> {{<property type : MULTI }}`

#### Deuxième rangée à N lignes

* La première colonne contient le chemin d’accès JCR absolu pour une ressource. Par exemple : /content/dam/asset1.jpg
* Il se peut que la propriété de métadonnées d’un fichier comporte des valeurs manquantes dans le fichier CSV. La propriété de métadonnées manquante pour cette ressource particulière ne sera pas mise à jour.