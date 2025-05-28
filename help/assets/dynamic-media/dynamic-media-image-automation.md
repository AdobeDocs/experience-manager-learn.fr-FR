---
title: Dynamic Media pour le traitement par lots de la transparence et de l’automatisation du contenu
description: Découvrez comment utiliser Dynamic Media dans AEM pour créer des rendus virtuels, gérer la transparence et automatiser le traitement des images en vue d’une réutilisation de contenu évolutive.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# Dynamic Media pour le traitement par lots de la transparence et de l’automatisation du contenu

Découvrez comment utiliser Dynamic Media dans AEM pour créer des rendus virtuels, gérer la transparence et automatiser le traitement des images en vue d’une réutilisation de contenu évolutive.

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Exemple de ressources Dynamic Media

Vous trouverez ci-dessous des exemples de ressources Dynamic Media et leurs URL utilisées dans la vidéo.

>[!BEGINTABS]

>[!TAB Exemples de transparence d’image]

Voici les exemples d’URL de serveur d’images Dynamic Media utilisés dans la vidéo.

| Prévisualisation | Description | URL Dynamic Media |
|-----------|------------------|---------|
| ![Par défaut](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Par défaut | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Composite avec un calque d’image d’arrière-plan transparent](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Composite avec couche d’image d’arrière-plan transparente | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Fond rouge](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | Arrière-plan rouge | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![Chemin d’accès réduit en ovale](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | Écrêté sur un chemin ovale | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB Exemples de chemins d’accès aux images]

Voici les exemples d’URL de serveur d’images Dynamic Media utilisés dans la vidéo.

| Prévisualisation | Description | URL Dynamic Media |
|-----------|------------------|---------|
| ![Normalisé à 80 pixels de large (pas de transparence)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Normalisé à 80 pixels de large (pas de transparence){width="250"} | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Recadrer sur le chemin](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | Recadrer sur le chemin | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![Écrêter au chemin](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | Écrêter sur le chemin | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![Clip sur le chemin et Recadrer sur le chemin](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | Écrêter sur le chemin et Recadrer sur le chemin | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![Écrêter sur un autre chemin](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | Écrêter sur un autre chemin | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![Écrêtez sur un autre chemin et faites un arrière-plan rouge](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | Écrêter à un autre chemin et faire un arrière-plan rouge | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## API du serveur d’images Dynamic Media

* [API de diffusion et de rendu d’images Dynamic Media](https://experienceleague.adobe.com/fr/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Aperçu d’instantané Dynamic Media](https://snapshot.scene7.com/)
