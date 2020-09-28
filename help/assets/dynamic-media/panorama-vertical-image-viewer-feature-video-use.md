---
title: Utilisation de la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media
seo-title: Utilisation de la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media
description: Les améliorations apportées à la visionneuse de médias dynamiques dans AEM version 6.4 incluent l’ajout de la visionneuse d’images panoramique, de la visionneuse d’images de réalité virtuelle panoramique et de la visionneuse d’images verticale. Panoramic Viewer offre un moyen facile de fournir une expérience attrayante et immersive de la chambre, de la propriété, de l'emplacement ou du paysage sans aucun développement personnalisé.
seo-description: Les améliorations apportées à la visionneuse de médias dynamiques dans AEM version 6.4 incluent l’ajout de la visionneuse d’images panoramique, de la visionneuse d’images de réalité virtuelle panoramique et de la visionneuse d’images verticale. Panoramic Viewer offre un moyen facile de fournir une expérience attrayante et immersive de la chambre, de la propriété, de l'emplacement ou du paysage sans aucun développement personnalisé.
sub-product: dynamic-media
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# Utilisation de la visionneuse d’images panoramiques et verticales avec AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Les améliorations apportées à la visionneuse de médias dynamiques dans AEM version 6.4 incluent l’ajout de la visionneuse d’images panoramique, de la visionneuse d’images de réalité virtuelle panoramique et de la visionneuse d’images verticale. Panoramic Viewer offre un moyen facile de fournir une expérience attrayante et immersive de la chambre, de la propriété, de l&#39;emplacement ou du paysage sans aucun développement personnalisé.

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>La vidéo suppose que votre instance AEM s’exécute en mode Contenu multimédia dynamique S7. [Vous trouverez ici les instructions relatives à la configuration des AEM avec Contenu multimédia dynamique.](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Visionneuse panoramique et panoramique VR

Une image est considérée comme panoramique en fonction de son format ou de ses mots-clés. Par défaut, une image avec un format de 2 est considérée comme une image panoramique. Les paramètres prédéfinis de la visionneuse d’images panoramiques seront disponibles pour une prévisualisation d’images si elle répond aux critères ci-dessus. Le critère du format d’image panoramique peut être modifié dans la configuration DMS7 de la société en spécifiant la propriété de doublon s7PanoramicAR à l’adresse /conf/global/settings/cloudconfigs/dmscene7/jcr:content. Les mots-clés sont stockés dans la propriété dc:keyword du noeud de métadonnées de la ressource. Si les mots-clés contiennent l&#39;une des combinaisons suivantes :

* équirectangulaire,
* sphérique + panoramique,
* panorama sphérique + panorama,

il sera considéré comme un fichier d’image panoramique, quel que soit son format.

## Visionneuse d’images verticale

Avec des nuances horizontales, en fonction de la taille de l’écran de bureau du consommateur, il arrive que les nuances ne soient pas visibles tant que l’utilisateur ne fait pas défiler la page. En utilisant la visionneuse d’images verticale et en plaçant des nuances verticales, vous vous assurez que les nuances sont visibles quelle que soit la taille de l’écran. Elle optimise également la taille d’image principale. Avec les nuances horizontales, il était nécessaire de réserver de l’espace sur la page pour s’assurer qu’elles ont une forte probabilité d’être visibles et qu’elles diminueraient la taille de l’image principale. Avec une disposition verticale, vous n’avez pas à vous soucier de l’allocation de cet espace et pouvez donc maximiser la taille de l’image principale.

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>Visionneuse panoramique et VR</td>
   <td>Visionneuse d’images verticale</td>
  </tr>
  <tr>
   <td>Mode d’exécution du média dynamique</td>
   <td>Mode Scene7 du média dynamique uniquement</td>
   <td>DMS7 et Contenu multimédia dynamique</td>
  </tr>
  <tr>
   <td>Exemple d’utilisation </td>
   <td><p>Le lecteur panoramique et le lecteur Virtual Reality offrent aux utilisateurs une expérience plus attrayante. Un utilisateur peut retirer une chambre d'hôtel avant même de faire une réservation ou bien vérifier un bien de location sans avoir à planifier de rendez-vous. Un utilisateur peut consulter un emplacement et de nombreuses autres possibilités. Le Principal objectif ici est de fournir aux consommateurs une meilleure expérience lorsqu'ils visitent votre site Web et éventuellement d'augmenter votre taux de conversion.</p> <p> </p> </td> 
   <td><p>La visionneuse d’images verticales permet d’optimiser l’expérience d’affichage des images des produits afin de fournir aux consommateurs la meilleure représentation du produit, ce qui entraîne des conversions et réduit les retours.</p> <p> </p> </td>
  </tr>
  <tr>
   <td>Disponible </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Configuration de Contenu multimédia dynamique en mode Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[Configuration de Contenu multimédia dynamique en mode hybride](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>Les visionneuses panoramiques fonctionnent avec des images panoramiques et ne sont pas destinées à être utilisées avec des images normales.
