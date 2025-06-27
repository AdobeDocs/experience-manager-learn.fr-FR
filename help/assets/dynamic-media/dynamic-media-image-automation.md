---
title: Dynamic Media pour le traitement par lots de la transparence et de l’automatisation du contenu
description: Découvrez comment utiliser Dynamic Media dans AEM pour créer des rendus virtuels, gérer la transparence et automatiser le traitement des images en vue d’une réutilisation de contenu évolutive.
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
exl-id: 13a09ad3-bf55-4524-bf43-f1cdad368034
source-git-commit: 1061a13089e5891ee375c959a9361079c0f8ce20
workflow-type: ht
source-wordcount: '262'
ht-degree: 100%

---

# Dynamic Media pour le traitement par lots de la transparence et de l’automatisation du contenu

Découvrez comment utiliser Dynamic Media dans AEM pour créer des rendus virtuels, gérer la transparence et automatiser le traitement des images en vue d’une réutilisation de contenu évolutive.

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Exemple de ressources Dynamic Media

Vous trouverez ci-dessous des exemples de ressources Dynamic Media et leurs URL utilisées dans la vidéo.

>[!BEGINTABS]

>[!TAB Exemples de transparence d’image]

Vous trouverez ci-dessous des exemples d’URL de diffusion d’images Dynamic Media utilisés dans la vidéo.

| Prévisualisation | Description | URL Dynamic Media |
|-----------|------------------|---------|
| ![Par défaut](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | Par défaut | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![Composé d’un calque d’image d’arrière-plan transparent](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&src=backdrop5-Camera&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans){width="250"} | Composé d’un calque d’image d’arrière-plan transparente | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&src=backdrop5-Camera&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans) |
| ![Arrière-plan rouge](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&color=200,50,50&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans){width="250"} | Arrière-plan rouge | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&color=200,50,50&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans) |
| ![Découpé selon un tracé oval](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&bgc=255,255,255){width="250"} | Découpé selon un tracé oval | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&bgc=255,255,255) |


>[!TAB Exemples de tracés d’image]

Vous trouverez ci-dessous des exemples d’URL de diffusion d’images Dynamic Media utilisés dans la vidéo.

| Prévisualisation | Description | URL Dynamic Media |
|-----------|------------------|---------|
| ![Normalisé à 80 pixels de large (pas de transparence)](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | Normalisé à 80 pixels de large (pas de transparence){width="250"} | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![Recadrer selon le tracé](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&wid=800){width="250"} | Recadrer selon le tracé | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&wid=800) |
| ![Découper selon le tracé](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&wid=800){width="250"} | Découper selon le tracé | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&wid=800) |
| ![Découper selon le tracé et recadrer selon le tracé](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&cropPathE=Path%201&wid=800){width="250"} | Découper selon le tracé et recadrer selon le tracé | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&cropPathE=Path%201&wid=800) |
| ![Découper selon un autre tracé](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&wid=800){width="250"} | Découper selon un autre tracé | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&wid=800) |
| ![Découper selon un autre tracé et ajouter un arrière-plan rouge](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&clipPathE=round&bgc=200,50,50&wid=800){width="250"} | Découper selon un autre tracé et ajouter un arrière-plan rouge | [Lien](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&clipPathE=round&bgc=200,50,50&wid=800) |

>[!ENDTABS]


## API de diffusion d’images Dynamic Media

* [API Diffusion et Rendu dʼimages Dynamic Media](https://experienceleague.adobe.com/fr/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Prévisualisation de l’instantané Dynamic Media](https://snapshot.scene7.com/)
