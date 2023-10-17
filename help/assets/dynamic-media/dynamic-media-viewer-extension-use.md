---
title: Utiliser des visionneuses Dynamic Media avec Adobe Analytics et Adobe Launch
description: L’extension Visionneuses Dynamic Media pour Adobe Launch, ainsi que la version 5.13 des Visionneuses Dynamic Media, permettent aux clientes et clients de Dynamic Media, Adobe Analytics et Adobe Launch d’utiliser des événements et des données spécifiques aux visionneuses Dynamic Media dans leur configuration Adobe Launch.
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '360'
ht-degree: 100%

---

# Utiliser des visionneuses Dynamic Media avec Adobe Analytics et Adobe Launch{#using-dynamic-media-viewers-adobe-analytics-launch}

Pour les clientes et les clients utilisant Dynamic Media et Adobe Analytics, vous pouvez désormais suivre l’utilisation des visionneuses Dynamic Media sur votre site web à l’aide de l’extension de visionneuse Dynamic Media.

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> Exécutez Adobe Experience Manager en mode Dynamic Media Scene7 pour cette fonctionnalité. Vous devez également [intégrer Adobe Experience Platform Launch à votre instance AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=fr).

Avec l’introduction de l’extension Visionneuse Dynamic Media, Adobe Experience Manager offre désormais une prise en charge avancée des fichiers fournis avec les visionneuses Dynamic Media (5.13), ce qui permet de contrôler plus précisément le suivi des événements lorsqu’une visionneuse Dynamic Media est utilisée sur une page Sites.

Si vous disposez déjà d’AEM Assets et de Sites, vous pouvez intégrer votre propriété Launch à votre instance de création d’AEM. Une fois l’intégration de lancement associée à votre site web, vous pouvez ajouter un composant Dynamic Media à votre page en activant le suivi des événements pour les visionneuses.

Pour les clientes et clients utilisant uniquement AEM Assets ou Dynamic Media Classic, il est possible d’obtenir le code incorporé d’une visionneuse et de l’ajouter à la page. Les bibliothèques de script Launch peuvent ensuite être ajoutées manuellement à la page pour le suivi des événements de la visionneuse.

Le tableau suivant répertorie les événements de visionneuse Dynamic Media et leurs arguments pris en charge :

<table>
   <tbody>
      <tr>
         <td>Nom de l’événement de visionneuse</td>
         <td>Référence de l’argument</td>
      </tr>
      <tr>
         <td> COMMON </td>
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
         <td> BANNER <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> ITEM </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> LOAD </td>
         <td> %event.detail.dm.LOAD.applicationame% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
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
         <td> METADATA </td>
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
         <td> PLAY </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> STOP </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> SWATCH </td>
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

* [Intégration d’Adobe Experience Manager à Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=fr)
* [Exécution d’Adobe Experience Manager en mode Dynamic Media Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=fr)
* [Intégration des visionneuses Dynamic Media à Adobe Analytics et Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html?lang=fr)
