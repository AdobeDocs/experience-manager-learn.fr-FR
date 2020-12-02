---
title: Utilisation de vidéos 360 et de miniatures de vidéos personnalisées dans Dynamic Media avec AEM Assets
seo-title: Utilisation de vidéos 360 et de miniatures de vidéos personnalisées dans Dynamic Media avec AEM Assets
description: Les améliorations apportées à la visionneuse de médias dynamiques dans AEM version 6.5 incluent la prise en charge du rendu vidéo 360, 360 visionneuses de médias (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.
seo-description: Les améliorations apportées à la visionneuse de médias dynamiques dans AEM version 6.5 incluent la prise en charge du rendu vidéo 360, 360 visionneuses de médias (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# Utilisation de vidéos 360 et de miniatures de vidéos personnalisées dans Dynamic Media avec AEM Assets

Les améliorations apportées à la visionneuse de médias dynamiques dans AEM version 6.5 incluent la prise en charge du rendu vidéo 360, 360 visionneuses de médias (video360Social et video360VR) et la possibilité de sélectionner des miniatures vidéo personnalisées.

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>La vidéo suppose que votre instance AEM s’exécute en mode Contenu multimédia dynamique S7.  [Vous trouverez ici](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html) des instructions sur la configuration des AEM avec Contenu multimédia dynamique. Lorsque vous téléchargez une vidéo, par défaut, Contenu multimédia dynamique traite le métrage sous la forme d’une vidéo de 360, si son rapport L/H est de 2:1. c&#39;est-à-dire que le rapport largeur/hauteur est de 2:1.

>[!NOTE]
>
>Les composants Contenu multimédia dynamique 360 prennent uniquement en charge 360 vidéos.

## Vidéos de Contenu multimédia dynamique 360

Les vidéos à 360 degrés, également connues sous le nom de vidéos sphériques, sont des enregistrements vidéo où une vue dans toutes les directions est enregistrée en même temps, filmée à l&#39;aide d&#39;une caméra omnidirectionnelle ou d&#39;une collection de caméras. Lors de la lecture sur un écran plat, l’utilisateur contrôle la direction d’affichage et la lecture sur des périphériques mobiles utilise généralement un contrôle gyroscopique intégré.  Il vous permet de dépasser les limites de la photographie unique. Les marketeurs peuvent offrir aux utilisateurs une expérience conviviale à l’aide de 360 vidéos.  Commençons. Le critère du format d’image panoramique peut être modifié dans la configuration DMS7 de la société en spécifiant la propriété de doublon s7PanoramicAR à l’adresse /conf/global/settings/cloudconfigs/dmscene7/jcr:content.

## Vidéos de Contenu multimédia dynamique 360

La vidéo Contenu multimédia dynamique prend désormais en charge la possibilité de sélectionner une miniature personnalisée pour votre vidéo. Un utilisateur peut sélectionner un fichier existant en AEM Assets ou sélectionner une image vidéo comme miniature.

## Visionneuses de supports dynamiques 360

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Visionneuse vidéo 360Social**</td>
      <td>**Visionneuse vidéo 360VR**</td>
   </tr>
   <tr>
      <td>Mode d’exécution du média dynamique</td>
      <td>Mode Scene7 du média dynamique uniquement</td>
      <td>Mode Scene7 du média dynamique uniquement<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>Exemple d’utilisation </td>
      <td>
         <p>Pour les sites Web et les périphériques qui ne prennent pas en charge le gyroscope</p>
         <p> </p>
      </td>
      <td>
         <p>Fournit une expérience de réalité virtuelle pour un périphérique qui prend en charge le gyroscope </p>
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
      <td>Point de navigation de la vue</td>
      <td>
         <ul>
            <li>Glissement de la souris (sur les systèmes de bureau)</li>
            <li>Glissement (périphériques tactiles)</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>Les options de souris et de glisser sont désactivées.</li>
            <li>Utilise un gyroscope intégré</li>
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

[Configuration de Contenu multimédia dynamique en mode Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
