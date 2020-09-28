---
title: Utilisation du lecteur vidéo dans AEM Dynamic Media
seo-title: Utilisation du lecteur vidéo dans AEM Dynamic Media
description: aem lecteur vidéo Contenu multimédia dynamique utilisé pour l’exécution en Flash pour la prise en charge de la diffusion de vidéo adaptative en flux continu sur les clients de bureau et les navigateurs sont devenus plus agressifs en cas de diffusion de contenu Flash en flux continu. Avec l'introduction de HLS (le protocole HTTP Live Streaming video diffusion d'Apple), le contenu peut désormais être diffusé en continu sans avoir besoin de Flash.
seo-description: aem lecteur vidéo Contenu multimédia dynamique utilisé pour l’exécution en Flash pour la prise en charge de la diffusion de vidéo adaptative en flux continu sur les clients de bureau et les navigateurs sont devenus plus agressifs en cas de diffusion de contenu Flash en flux continu. Avec l'introduction de HLS (le protocole HTTP Live Streaming video diffusion d'Apple), le contenu peut désormais être diffusé en continu sans avoir besoin de Flash.
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: dynamic-media
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 29%

---


# Utilisation du lecteur vidéo dans AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

aem lecteur vidéo Contenu multimédia dynamique utilisé pour l’exécution en Flash pour la prise en charge de la diffusion de vidéo adaptative en flux continu sur les clients de bureau et les navigateurs sont devenus plus agressifs en cas de diffusion de contenu Flash en flux continu. Avec l&#39;introduction de HLS (le protocole HTTP Live Streaming video diffusion d&#39;Apple), le contenu peut désormais être diffusé en continu sans avoir besoin de Flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Aperçu rapide du lecteur vidéo non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

La prise en charge du navigateur HLS est la suivante : pour les navigateurs non pris en charge, nous abandonnons la diffusion vidéo progressive.

<table> 
 <thead> 
  <tr> 
   <th> <p>Appareil</p> </th>
   <th> <p>Navigateur</p> </th>
   <th > <p>Mode lecture vidéo</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Internet Explorer 9 et 10</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr>
   <td> <p>Poste de travail</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Diffusion vidéo en flux continu HLS</p> </td>
  </tr>
  <tr>
   <td> <p>Poste de travail</p> </td>
   <td> <p>Firefox 23 à 44</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Firefox 45 ou version ultérieure</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 6 ou version antérieure)</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 7 ou version ultérieure)</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Android (navigateur par défaut)</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS diffusion vidéo</p> </td>
  </tr>
 </tbody>
</table>