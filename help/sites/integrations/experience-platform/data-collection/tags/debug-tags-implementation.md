---
title: Débogage d’une implémentation de balises
description: Cette section présente certains outils et techniques courants pour déboguer une implémentation de balises. Découvrez comment utiliser la console de développement du navigateur et l’extension Experience Platform Debugger pour identifier et résoudre les principaux problèmes liés à une implémentation des balises.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Débogage d’une implémentation de balises {#debug-tags-implementation}

Cette section présente les outils et techniques courants utilisés pour déboguer une implémentation de balises. Découvrez comment utiliser la console de développement du navigateur et l’extension Experience Platform Debugger pour identifier et résoudre les principaux problèmes liés à une implémentation des balises.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Débogage côté client via un objet satellite

Le débogage côté client s’avère utile pour vérifier le chargement des règles de propriété de balise ou l’ordre d’exécution. Chaque fois qu’une propriété Tag est ajoutée au site web, la variable `_satellite` L’objet JavaScript est présent dans le navigateur pour faciliter le suivi des événements et des données côté client.

Pour activer le débogage côté client, appelez la fonction `setDebug(true)` sur la méthode `_satellite` .

1. Ouvrez la console du navigateur et exécutez la commande ci-dessous.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Rechargez la page du site AEM et vérifiez que le journal de la console s’affiche. _déclenchement de règle_ comme ci-dessous.

   ![Propriété de balise sur les pages de création et de publication](assets/satellite-object-debugging.png)

## Débogage via Adobe Experience Platform Debugger

Adobe fournit un Adobe Experience Platform Debugger [Extension Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) et [Module complémentaire Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) pour déboguer, comprendre et obtenir des informations sur l’intégration.

1. Ouvrez l’extension Adobe Experience Platform Debugger et ouvrez la page du site sur l’instance de publication.

1. Dans le **Adobe Experience Platform Debugger > Résumé > Balises Adobe Experience Platform** , vérifiez les détails de la propriété de balise tels que le nom, la version, la date de création, l’environnement et les extensions.

   ![Détails de la propriété Adobe Experience Platform Debugger et Tag](assets/tag-property-details.png)

## Ressources supplémentaires {#additional-resources}

+ [Présentation de l’Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Référence d’objet satellite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
