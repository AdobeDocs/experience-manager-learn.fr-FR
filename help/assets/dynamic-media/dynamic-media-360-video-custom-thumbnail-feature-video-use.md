---
title: Utilisation de vidéos Dynamic Media 360 et de miniatures vidéo personnalisées avec AEM Assets
description: Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.5 incluent la prise en charge du rendu vidéo 360, des visionneuses de médias 360 (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.
sub-product: dynamic-media
feature: Profils vidéo
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 5%

---


# Utilisation de vidéos Dynamic Media 360 et de miniatures vidéo personnalisées avec AEM Assets

Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.5 incluent la prise en charge du rendu vidéo 360, des visionneuses de médias 360 (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>La vidéo suppose que votre instance AEM est en cours d’exécution en mode Dynamic Media S7.  [Vous trouverez des instructions sur la configuration d’AEM avec Dynamic Media ici](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Lorsque vous téléchargez une vidéo, Dynamic Media traite par défaut la vidéo sous la forme d’une vidéo 360, avec un rapport largeur/hauteur de 2:1. c’est-à-dire que le rapport largeur/hauteur est de 2:1.

>[!NOTE]
>
>Les composants Dynamic Media 360 Media prennent uniquement en charge les vidéos 360.

## Vidéos Dynamic Media 360

Les vidéos 360 degrés, également appelées vidéos sphériques, sont des enregistrements vidéo où une vue dans toutes les directions est enregistrée en même temps, tournée à l’aide d’une caméra omnidirectionnelle ou d’une collection de caméras. Lors de la lecture sur un écran plat, l’utilisateur contrôle la direction de l’affichage, tandis que la lecture sur les appareils mobiles utilise généralement une commande de gyroscope intégrée.  Il vous permet de vous étendre au-delà des limites de la photographie unique. Les marketeurs peuvent offrir aux utilisateurs une expérience attrayante grâce à des vidéos 360.  Commençons. Le critère de format d’image panoramique peut être modifié dans la configuration DMS7 de l’entreprise en spécifiant la propriété double s7PanoramicAR à /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vidéos Dynamic Media 360

La vidéo Dynamic Media prend désormais en charge la possibilité de sélectionner une miniature personnalisée pour votre vidéo. Un utilisateur peut sélectionner une ressource existante dans AEM Assets ou sélectionner une image vidéo comme miniature.

## Visionneuses Dynamic 360 Media

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Visionneuse de vidéos 360Social**</td>
      <td>**Visionneuse Video360VR**</td>
   </tr>
   <tr>
      <td>Mode d’exécution Dynamic Media</td>
      <td>Mode Scene7 Dynamic Media uniquement</td>
      <td>Mode Scene7 Dynamic Media uniquement<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Exemple d’utilisation </td>
      <td>
         <p>Pour les sites web et les appareils qui ne prennent pas en charge le gyroscope</p>
         <p> </p>
      </td>
      <td>
         <p>Offre une expérience de réalité virtuelle à un appareil prenant en charge le gyroscope </p>
      </td>
   </tr>
   <tr>
      <td>Audio - Mode stéréo</td>
      <td>Non</td>
      <td>Oui</td>
   </tr>
   <tr>
      <td>Lecture vidéo</td>
      <td>Oui</td>
      <td>Oui</td>
   </tr>
   <tr>
      <td>Navigation dans le point de vue</td>
      <td>
         <ul>
            <li>Faites glisser la souris (sur les systèmes de bureau)</li>
            <li>Balayage (appareils tactiles)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Les options de souris et de glisser sont désactivées.</li>
            <li>Utilisation d’un gyroscope intégré</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Lecteur HTML5</td>
      <td>Oui</td>
      <td>Oui</td>
   </tr>
   <tr>
      <td>Options de partage sur les réseaux sociaux</td>
      <td>Oui</td>
      <td>Non</td>
   </tr>
</tbody>
</table>

## Ressources supplémentaires{#additional-resources}

[Configuration de Dynamic Media en mode Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
