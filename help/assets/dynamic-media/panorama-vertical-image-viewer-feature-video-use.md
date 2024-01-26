---
title: Utiliser la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media
description: Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.4 incluent l’ajout de plusieurs visionneuses (images panoramiques, images de réalité virtuelle panoramique et images verticales). La visionneuse panoramique procure un moyen simple d’offrir une expérience immersive et attrayante de la pièce, de la propriété, de l’emplacement ou du paysage sans aucun développement personnalisé.
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 563
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 100%

---

# Utiliser la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.4 incluent l’ajout de plusieurs visionneuses (images panoramiques, images de réalité virtuelle panoramique et images verticales). La visionneuse panoramique procure un moyen simple d’offrir une expérience immersive et attrayante de la pièce, de la propriété, de l’emplacement ou du paysage sans aucun développement personnalisé.

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>Cette vidéo part du principe que votre instance AEM est en cours d’exécution en mode Dynamic Media S7. [Vous trouverez des instructions sur la configuration d’AEM avec Dynamic Media ici.](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visionneuse panoramique avec et sans réalité virtuelle

Une image est considérée comme panoramique en fonction de son format ou de ses mots-clés. Par défaut, une image avec un format de 2 est considérée comme une image panoramique. Les paramètres prédéfinis de la visionneuse d’images panoramiques permettent l’aperçu d’une image si elle répond aux critères cités. Le critère de format d’image panoramique peut être modifié dans la configuration DMS7 de l’entreprise en spécifiant la propriété double s7PanoramicAR à /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Les mots-clés sont stockés dans la propriété dc:keyword du nœud de métadonnées du fichier. Si les mots-clés contiennent l’une des combinaisons suivantes :

* équirectangulaire,
* sphérique+panoramique,
* sphérique+panorama,

le fichier est alors considéré comme une image panoramique, quel que soit son format.

## Visionneuse d’images verticales

Lorsqu’ils sont horizontaux, en fonction de la taille de l’écran du poste de travail de l’utilisateur ou de l’utilisatrice, il arrive que les nuanciers ne soient pas visibles tant que l’utilisateur ou l’utilisatrice ne fait pas défiler la page vers le bas. En utilisant la visionneuse d’images verticales avec des nuanciers verticaux, vous vous assurez qu’ils sont visibles quelle que soit la taille de l’écran. Elle optimise également la taille de l’image principale. Avec les nuanciers horizontaux, il était nécessaire de réserver de l’espace sur la page pour assurer leur visibilité, mais aussi pour réduire la taille de l’image principale. Avec une disposition verticale, il n’est pas nécessaire de se soucier de l’attribution de cet espace, ni de comment maximiser la taille de l’image principale.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visionneuse panoramique et de réalité virtuelle</td>
   <td>Visionneuse d’images verticales</td>
  </tr>
  <tr>
   <td>Mode d’exécution Dynamic Media</td>
   <td>Mode Dynamic Media Scene7 uniquement</td>
   <td>DMS7 et Dynamic Media</td>
  </tr>
  <tr>
   <td>Cas d’utilisation</td>
   <td><p>La visionneuse panoramique et la visionneuse de réalité virtuelle offrent aux utilisateurs et aux utilisatrices une expérience plus attrayante. L’utilisateur ou l’utilisatrice peut voir une chambre d’hôtel avant même de la réserver ou une location sans avoir à planifier de rendez-vous. L’utilisateur ou l’utilisatrice peut consulter un emplacement et de nombreuses autres possibilités. L’objectif principal ici est de fournir aux clientes et clients une meilleure expérience lorsqu’ils visitent votre site web afin d’augmenter votre taux de conversion en conséquence.</p> <p> </p> </td> 
   <td><p>La visionneuse d’images verticales permet d’optimiser l’expérience d’affichage du produit afin de fournir aux consommateurs et aux consommatrices la meilleure représentation du produit pour augmenter vos conversions et réduire vos retours.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponible </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configurer Dynamic Media en mode Scene7](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/config-dms7.html)

[Configurer Dynamic Media en mode hybride](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Les visionneuses panoramiques fonctionnent avec des images panoramiques et ne sont pas destinées à être utilisées avec des images normales.
