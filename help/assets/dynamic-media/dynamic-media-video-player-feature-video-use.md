---
title: Utilisation du lecteur vidéo dans AEM Dynamic Media
description: AEM lecteur vidéo Dynamic Media utilisait le runtime Flash pour prendre en charge la diffusion en continu de vidéo adaptative sur les clients de bureau et les navigateurs sont devenus plus agressifs à l’aide de la diffusion en continu de contenu Flash. Avec l’introduction de HLS (protocole de diffusion vidéo HTTP Live Streaming Apple), le contenu peut désormais être diffusé en continu sans recourir au flash.
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: 947c280f32b013a6ade76b2f3df1152b29108c6e
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 31%

---


# Utilisation du lecteur vidéo dans AEM Dynamic Media{#using-the-video-player-in-aem-dynamic-media}

AEM lecteur vidéo Dynamic Media utilisait le runtime Flash pour prendre en charge la diffusion en continu de vidéo adaptative sur les clients de bureau et les navigateurs sont devenus plus agressifs à l’aide de la diffusion en continu de contenu Flash. Avec l’introduction de HLS (protocole de diffusion vidéo HTTP Live Streaming Apple), le contenu peut désormais être diffusé en continu sans recourir au flash.

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Recherche rapide dans un lecteur vidéo non Flash {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

La prise en charge des navigateurs HLS est la suivante : pour les navigateurs non pris en charge, nous abandonnons la diffusion vidéo progressive.

>[!NOTE]
>
> Dynamic Media Hybrid NE prendra PAS en charge la diffusion vidéo en continu sur Internet Explorer 11 après le 15 mars 2022.

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
   <td> <p>Diffusion vidéo en continu HLS</p> </td>
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
   <td> <p>Diffusion vidéo en continu HLS</p> </td>
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
