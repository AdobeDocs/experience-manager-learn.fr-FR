---
title: Utilisation de la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media
description: Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.4 incluent l’ajout de la visionneuse d’images panoramiques, de la visionneuse d’images de réalité virtuelle panoramique et de la visionneuse d’images verticales. La visionneuse panoramique offre un moyen simple d’offrir une expérience immersive et attrayante de la pièce, de la propriété, de l’emplacement ou du paysage sans aucun développement personnalisé.
sub-product: dynamic-media
feature: Profils vidéo, profils vidéo, vidéo 360 VR
version: 6.4, 6.5
topic: Gestion de contenu
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 3%

---


# Utilisation de la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.4 incluent l’ajout de la visionneuse d’images panoramiques, de la visionneuse d’images de réalité virtuelle panoramique et de la visionneuse d’images verticales. La visionneuse panoramique offre un moyen simple d’offrir une expérience immersive et attrayante de la pièce, de la propriété, de l’emplacement ou du paysage sans aucun développement personnalisé.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>La vidéo suppose que votre instance AEM est en cours d’exécution en mode Dynamic Media S7. [Vous trouverez des instructions sur la configuration d’AEM avec Dynamic Media ici.](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visionneuse de réalité virtuelle panoramique et panoramique

Une image est considérée comme panoramique en fonction de ses proportions ou de ses mots-clés. Par défaut, une image avec un format de 2 sera considérée comme une image panoramique. Les paramètres prédéfinis de la visionneuse d’images panoramiques deviennent disponibles pour un aperçu d’image s’il répond aux critères ci-dessus. Le critère de format d’image panoramique peut être modifié dans la configuration DMS7 de l’entreprise en spécifiant la propriété double s7PanoramicAR à /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Les mots-clés sont stockés dans la propriété dc:keyword du noeud de métadonnées de la ressource. Si les mots-clés contiennent l’une des combinaisons suivantes :

* équirectangulaire,
* sphérique + panoramique,
* sphérique + panorama,

il sera considéré comme une ressource Image panoramique, quel que soit son format.

## Visionneuse d’images verticale

Avec des échantillons horizontaux, en fonction de la taille de l’écran de bureau du consommateur, les échantillons ne sont parfois pas visibles tant que l’utilisateur ne fait pas défiler la page vers le bas. En utilisant la visionneuse d’images verticale et en plaçant des échantillons verticaux, vous avez la garantie que les échantillons sont visibles quelle que soit la taille de l’écran. Il optimise également la taille de l’image principale. Avec les échantillons horizontaux, il était nécessaire de réserver de l’espace sur la page pour s’assurer qu’ils ont une forte probabilité d’être visibles et qu’ils réduiraient la taille de l’image principale. Avec une disposition verticale, il n’est pas nécessaire de se soucier de l’allocation de cet espace et donc de maximiser la taille de l’image principale.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visionneuse panoramique et VR</td>
   <td>Visionneuse d’images verticale</td>
  </tr>
  <tr>
   <td>Mode d’exécution Dynamic Media</td>
   <td>Mode Scene7 Dynamic Media uniquement</td>
   <td>DMS7 et Dynamic Media</td>
  </tr>
  <tr>
   <td>Exemple d’utilisation </td>
   <td><p>La visionneuse panoramique et la visionneuse de réalité virtuelle offrent aux utilisateurs une expérience plus attrayante. Un utilisateur peut vérifier une chambre d'hôtel avant même qu'il ne fasse sa réservation ou bien vérifier un bien de location sans avoir à planifier de rendez-vous. Un utilisateur peut consulter un emplacement et de nombreuses autres possibilités. L’objectif Principal ici est de fournir aux clients une meilleure expérience lorsqu’ils visitent votre site web et éventuellement d’augmenter votre taux de conversion.</p> <p> </p> </td> 
   <td><p>La visionneuse d’images verticales permet d’optimiser l’expérience d’affichage de l’imagerie du produit afin de fournir aux consommateurs la meilleure représentation du produit, ce qui entraîne une conversion et minimise les retours.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponible </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configuration de Dynamic Media en mode Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configuration de Dynamic Media en mode hybride](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Les visionneuses panoramiques fonctionnent avec des images panoramiques et ne sont pas destinées à être utilisées avec des images normales.
