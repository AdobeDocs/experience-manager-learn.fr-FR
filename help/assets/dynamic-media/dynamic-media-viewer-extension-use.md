---
title: Utilisation des visionneuses Dynamic Media avec Adobe Analytics et lancement d’Adobe
description: L’extension Visionneuses Dynamic Media pour Adobe Launch, ainsi que la version 5.13 des Visionneuses Dynamic Media, permettent aux clients de Dynamic Media, Adobe Analytics et Adobe Launch d’utiliser des événements et des données spécifiques aux visionneuses Dynamic Media dans leur configuration Adobe Launch.
sub-product: Dynamic Media
feature: Statistiques sur les ressources
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Professionnel
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 26%

---


# Utilisation des visionneuses Dynamic Media avec Adobe Analytics et lancement d’Adobe{#using-dynamic-media-viewers-adobe-analytics-launch}

Pour les clients disposant de Dynamic Media et Adobe Analytics, vous pouvez désormais suivre l’utilisation des visionneuses Dynamic Media sur votre site Web à l’aide de l’extension Dynamic Media Viewer.

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> Exécutez Adobe Experience Manager en mode Scene7 pour cette fonctionnalité. Vous devez également [intégrer Adobe Experience Platform Launch à votre instance AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html).

Avec l’introduction de l’extension Dynamic Media Viewer, Adobe Experience Manager offre désormais la prise en charge avancée des analyses pour les ressources fournies avec les visionneuses Dynamic Media (5.13), ce qui permet un contrôle plus précis sur le suivi des événements lorsqu’une visionneuse Dynamic Media est utilisée sur une page de sites.

Si vous disposez déjà d’AEM Assets et de Sites, vous pouvez intégrer votre propriété Launch à votre instance d’auteur AEM. Une fois que l’intégration de votre lancement est associée à votre site Web, vous pouvez ajouter un composant multimédia dynamique à votre page avec le suivi de événement activé pour les visionneuses.

Pour les clients AEM Assets uniquement ou Dynamic Media Classic, l’utilisateur peut obtenir le code incorporé d’une visionneuse et l’ajouter à la page. Les bibliothèques de script de lancement peuvent ensuite être ajoutées manuellement à la page pour le suivi des événements de la visionneuse.

Le tableau suivant répertorie les événements de visionneuse Dynamic Media et leurs arguments pris en charge :

<table>
   <tbody>
      <tr>
         <td>Nom de l’événement de visionneuse</td>
         <td>Référence de l’argument</td>
      </tr>
      <tr>
         <td> COMMUN </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> BANNIÈRE <br></td>
         <td> %event.detail.dm.BANER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ELEMENT </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> CHARGEMENT </td>
         <td> %event.detail.dm.LOAD.applicationame% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.société% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> MÉTADONNÉES </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> MILESTONE </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> PAGE </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUSE </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> LECTURE </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> ARRÊTER </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> ÉCHANTILLON </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ZOOM </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## Ressources supplémentaires{#additional-resources}

* [Intégration de Adobe Experience Manager avec le lancement d&#39;Adobe](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Exécution de Adobe Experience Manager en mode Scene7](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [Intégration de visionneuses de médias dynamiques à Adobe Analytics et Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
