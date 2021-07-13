---
title: Utilisation du recadrage intelligent avec AEM Assets Dynamic Media
description: Le recadrage intelligent utilise Adobe Sensei pour éliminer les tâches fastidieuses et coûteuses de recadrage de contenu pour une conception adaptée.
sub-product: dynamic-media
feature: Recadrage intelligent, profils d’image
version: 6.4, 6.5
topic: Gestion de contenu
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 18%

---


# Utilisation du recadrage intelligent avec AEM Assets Dynamic Media{#using-smart-crop-with-aem-assets-dynamic-media}

Le recadrage intelligent utilise Adobe Sensei pour éliminer les tâches fastidieuses et coûteuses de recadrage de contenu pour une conception adaptée.

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>La vidéo suppose que votre instance AEM est en cours d’exécution en mode Dynamic Media S7. [Vous trouverez des instructions sur la configuration d’AEM avec Dynamic Media ici.](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## La fonctionnalité de recadrage intelligent de Adobe Experience Manager Dynamic Media inclut

* AEM les administrateurs de ressources peuvent facilement créer des profils d’image pour le recadrage intelligent en fonction de la largeur et de la hauteur de l’appareil.
* Le recadrage intelligent peut être effectué pour une ressource individuelle ou pour toutes les ressources d’un dossier.
* Vous pouvez redimensionner la mise en page avec recadrage intelligent pour une meilleure visibilité.
* Le composant Dynamic Media d’AEM Sites prend en charge le recadrage intelligent.
* L’URL publiée de la ressource recadrée intelligente peut être utilisée avec les applications tierces qui acceptent les ressources hébergées.

>[!NOTE]
>
>Les coordonnées de recadrage intelligent dépendent des proportions. En d’autres termes, pour les différents paramètres de recadrage intelligent d’un profil d’image, si le rapport d’aspect est identique pour les dimensions ajoutées au profil d’image, ce même rapport d’aspect est envoyé à Dynamic media. C’est pourquoi la même zone de recadrage est suggérée dans l’éditeur de recadrage intelligent. Par exemple, un paramètre de recadrage de 100x100 et 200x200 entraînerait la génération du même recadrage intelligent par le système.
