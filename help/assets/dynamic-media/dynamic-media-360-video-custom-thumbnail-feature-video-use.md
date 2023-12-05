---
title: Utiliser les vidéos 360 Dynamic Media et les miniatures vidéo personnalisées avec AEM Assets
description: Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.5 incluent la prise en charge du rendu vidéo 360, des visionneuses de médias 360 (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 691
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 100%

---

# Utiliser les vidéos 360 Dynamic Media et les miniatures vidéo personnalisées avec AEM Assets

Les améliorations apportées à la visionneuse Dynamic Media dans AEM 6.5 incluent la prise en charge du rendu vidéo 360, des visionneuses de médias 360 (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>Cette vidéo part du principe que votre instance AEM est en cours d’exécution en mode Dynamic Media S7.  [Vous trouverez des instructions sur la configuration d’AEM avec Dynamic Media ici](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). Lorsque vous téléchargez une vidéo, Dynamic Media la traite par défaut comme une vidéo 360, si elle a un format de 2:1, c’est-à-dire un rapport largeur/hauteur de 2:1.

>[!NOTE]
>
>Les composants de médias 360 Dynamic Media prennent uniquement en charge les vidéos 360.

## Vidéos 360 Dynamic Media

Les vidéos à 360 degrés, également appelées vidéos sphériques, consistent en des enregistrements vidéo dans lesquels une vue dans toutes les directions est enregistrée en même temps. Elles sont filmées à l’aide de caméras omnidirectionnelles ou de plusieurs caméras. Lors de la lecture sur un écran plat, la personne utilisatrice contrôle elle-même l’angle de vue. Les appareils mobiles tirent généralement parti du gyroscope intégré.  Oubliez le champ de vision unique d’une photo et passez à une multitude de perspectives. Grâce aux vidéos à 360°, les personnes professionnelles du marketing peuvent offrir à leur audience une expérience stimulante.  Commençons. Le critère de format d’image panoramique peut être modifié dans la configuration DMS7 de l’entreprise en spécifiant la propriété double s7PanoramicAR à /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vidéos 360 Dynamic Media

Les vidéos Dynamic Media prennent désormais en charge les miniatures personnalisée pour vos vidéos. La personne utilisatrice peut choisir une ressource existante dans AEM Assets ou une image vidéo comme miniature.

## Visionneuses 360 Dynamic Media

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Visionneuse Video360Social**</td>
      <td>**Visionneuse Video360VR**</td>
   </tr>
   <tr>
      <td>Mode d’exécution Dynamic Media</td>
      <td>Mode Dynamic Media Scene7 uniquement</td>
      <td>Mode Dynamic Media Scene7 uniquement<br>
 <br>
      </td>
   </tr>
   <tr>
      <td>Cas d’utilisation</td>
      <td>
         <p>Pour les sites web et les appareils dépourvus de gyroscope.</p>
         <p> </p>
      </td>
      <td>
         <p>Offre une expérience de réalité virtuelle pour les appareils équipés d’un gyroscope.</p>
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
      <td>Navigation au niveau du point de vue</td>
      <td>
         <ul>
            <li>Glisser-déposer avec la souris (sur les systèmes de bureau)</li>
            <li>Balayage (appareils tactiles)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Les options de glisser-déposer avec la souris sont désactivées.</li>
            <li>Utilisation du gyroscope intégré</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>Lecteur HTML5</td>
      <td>Oui</td>
      <td>Oui</td>
   </tr>
   <tr>
      <td>Options de partage social</td>
      <td>Oui</td>
      <td>Non</td>
   </tr>
</tbody>
</table>

## Ressources supplémentaires{#additional-resources}

[Configurer Dynamic Media en mode Scene7](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/config-dms7.html)
