---
title: Utiliser le lecteur vidéo dans AEM Dynamic Media
description: Le lecteur vidéo AEM Dynamic Media utilisait l’exécution Flash pour prendre en charge le streaming de vidéo adaptative sur les clients de bureau et les navigateurs sont devenus plus agressifs avec le streaming de contenu Flash. Avec l’introduction de HLS (protocole de diffusion vidéo HTTP Live Streaming Apple), le contenu peut désormais être diffusé en continu sans recourir au Flash.
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 608
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '265'
ht-degree: 100%

---


# Utiliser le lecteur vidéo dans AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

Le lecteur vidéo AEM Dynamic Media utilisait l’exécution Flash pour prendre en charge le streaming de vidéo adaptative sur les clients de bureau et les navigateurs sont devenus plus agressifs avec le streaming de contenu Flash. Avec l’introduction de HLS (protocole de diffusion vidéo HTTP Live Streaming Apple), le contenu peut désormais être diffusé en continu sans recourir au Flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## Aperçu rapide d’un lecteur vidéo non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

La prise en charge des navigateurs HLS est la suivante : pour les navigateurs non pris en charge, nous abandonnons la diffusion vidéo progressive.

>[!NOTE]
>
> Dynamic Media Hybrid ne prend PAS en charge le streaming vidéo sur Internet Explorer 11 à compter du 15 mars 2022. Effectuez une mise à niveau vers la version 6.5.12 ou ultérieure pour revenir à la lecture progressive sur Internet Explorer 11.

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
   <td> <p>Internet Explorer 9 et 10</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr>
   <td> <p>Poste de travail</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media - Mode Scene 7 : streaming vidéo HLS</p> 
        <p>Dynamic Media - Mode hybride : téléchargement progressif</p>
   </td>
  </tr>
  <tr>
   <td> <p>Poste de travail</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Firefox 45 ou version ultérieure</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Poste de travail</p> </td>
   <td> <p>Safari (Mac)</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 6 ou version antérieure)</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (Android 7 ou version ultérieure)</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Android (navigateur par défaut)</p> </td>
   <td> <p>Téléchargement progressif</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Safari (iOS)</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Chrome (iOS)</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
  <tr> 
   <td> <p>Mobile</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>Streaming vidéo HLS</p> </td>
  </tr>
 </tbody>
</table>
