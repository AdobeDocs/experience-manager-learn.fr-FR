---
title: Déboguer une implémentation de balises
description: Cette section présente certains outils et techniques courants pour déboguer une implémentation de balises. Découvrez comment utiliser la console de développement du navigateur et l’extension Experience Platform Debugger pour identifier et résoudre les problèmes principaux liés à une implémentation des balises.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 272
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 100%

---

# Déboguer une implémentation de balises {#debug-tags-implementation}

Cette section présente les outils et techniques courants utilisés pour déboguer une implémentation de balises. Découvrez comment utiliser la console de développement du navigateur et l’extension Experience Platform Debugger pour identifier et résoudre les problèmes principaux liés à une implémentation des balises.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Débogage côté client via un objet satellite

Le débogage côté client s’avère utile pour vérifier le chargement ou l’ordre d’exécution des règles de propriété de balise. Chaque fois qu’une propriété de balise est ajoutée au site web, l’objet `_satellite` JavaScript est présent dans le navigateur pour faciliter le suivi des événements et des données côté client.

Pour activer le débogage côté client, appelez la méthode `setDebug(true)` sur l’objet `_satellite`.

1. Ouvrez la console du navigateur et exécutez la commande ci-dessous.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Rechargez la page du site AEM et vérifiez que le journal de la console affiche le message _déclenchement de règle_ comme ci-dessous.

   ![Propriété de balise sur les pages de création et de publication.](assets/satellite-object-debugging.png)

## Débogage via Adobe Experience Platform Debugger

Adobe fournit une [extension Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) et un [module complémentaire Firefox](https://addons.mozilla.org/fr/firefox/addon/adobe-experience-platform-dbg/) Adobe Experience Platform Debugger pour déboguer, comprendre et obtenir des informations sur l’intégration.

1. Ouvrez l’extension Adobe Experience Platform Debugger et ouvrez la page du site sur l’instance de publication.

1. Dans la section **Adobe Experience Platform Debugger > Résumé > Balises Adobe Experience Platform**, vérifiez les détails de la propriété de balise tels que le nom, la version, la date de création, l’environnement et les extensions.

   ![Détails de la propriété de balise et Adobe Experience Platform Debugger.](assets/tag-property-details.png)

## Ressources supplémentaires {#additional-resources}

+ [Présentation d’Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=fr)

+ [Référence d’objet satellite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html?lang=fr)
