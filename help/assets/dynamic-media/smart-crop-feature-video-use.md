---
title: Utilisation de la recadrage intelligente avec AEM Assets Dynamic Media
description: Smart Crop utilise Adobe Sensei pour éliminer les tâches fastidieuses et coûteuses de recadrage de contenu pour une conception adaptée.
sub-product: dynamic-media
feature: Recadrage dynamique, Profils d’image
version: 6.4, 6.5
topic: Gestion de contenu
role: Professionnel
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 17%

---


# Utilisation de la recadrage intelligente avec AEM Assets{#using-smart-crop-with-aem-assets-dynamic-media}

Smart Crop utilise Adobe Sensei pour éliminer les tâches fastidieuses et coûteuses de recadrage de contenu pour une conception adaptée.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>La vidéo suppose que votre instance AEM s’exécute en mode Dynamic Media S7. [Vous trouverez des instructions sur la configuration de l&#39;AEM avec Dynamic Media ici.](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## La fonctionnalité Adobe Experience Manager Smart Crop inclut

* Les administrateurs AEM ressources peuvent facilement créer des profils d’images pour un recadrage intelligent en fonction de la largeur et de la hauteur du périphérique.
* Le recadrage intelligent peut être effectué pour une ressource individuelle ou pour toutes les ressources d’un dossier.
* Vous pouvez redimensionner la mise en page de l’édition de recadrage dynamique pour une meilleure visibilité.
* Le composant Dynamic Media des sites AEM prend en charge la recadrage dynamique.
* L’URL publiée pour la ressource recadrée dynamique peut être utilisée avec des applications tierces qui acceptent des ressources hébergées.

>[!NOTE]
>
>Les coordonnées de recadrage intelligent dépendent du rapport L/H. En d’autres termes, pour les différents paramètres de recadrage intelligent d’un profil d’image, si le rapport d’aspect est identique pour les dimensions ajoutées au profil d’image, ce même rapport d’aspect est envoyé à Dynamic media. C’est pourquoi la même zone de recadrage sera suggérée dans l’éditeur de recadrage dynamique. Par exemple, un paramètre de recadrage de 100x100 et 200x200 entraînerait la génération de la même culture intelligente par le système.
