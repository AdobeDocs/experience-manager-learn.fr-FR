---
title: Débogage du SDK d’AEM à l’aide de la console Web OSGi
description: Le démarrage rapide local du SDK AEM dispose d’une console Web OSGi qui fournit un large éventail d’informations et d’introspections dans l’AEM d’exécution locale, utiles pour comprendre comment votre application est reconnue et fonctionne dans l’AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 3%

---


# Débogage du SDK d’AEM à l’aide de la console Web OSGi

Le démarrage rapide local du SDK AEM dispose d’une console Web OSGi qui fournit un large éventail d’informations et d’introspections dans l’AEM d’exécution locale, utiles pour comprendre comment votre application est reconnue et fonctionne dans l’AEM.

aem fournit de nombreuses consoles OSGi, chacune fournissant des informations clés sur différents aspects de l&#39;AEM, mais les éléments suivants sont généralement les plus utiles pour déboguer votre application.

## Lots

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

La console Bundles est un catalogue des lots OSGi et de leurs détails, déployés sur AEM, ainsi que la capacité ad hoc de les début et de les arrêter.

La console Bundles se trouve à l’emplacement suivant :

+ Outils > Opérations > Console Web > OSGi > Bundles
+ Ou directement à : [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Cliquez sur chaque lot pour afficher des informations détaillées sur le débogage de votre application.

+ La validation du lot OSGi est présente
+ Validation si un lot OSGi est principal
+ Détermination de la capacité d’importation d’un lot OSGi insatisfaite empêchant son démarrage

## Composants

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

La console Composants est un catalogue de tous les composants OSGi déployés sur AEM et fournit toutes les informations les concernant, depuis leur cycle de vie de composants OSGi défini jusqu&#39;aux services OSGi auxquels ils peuvent faire référence.

La console Composants se trouve à l’emplacement suivant :

+ Outils > Opérations > Console Web > OSGi > Composants
+ Ou directement à : [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Aspects clés qui aident à exécuter les activités de débogage :

+ La validation du lot OSGi est présente
+ Validation si un lot OSGi est principal
+ Détermination de la capacité d’importation d’un lot OSGi insatisfaite empêchant son démarrage
+ Obtention du PID du composant, afin de créer des configurations OSGi pour eux en mode Git
+ Identification des valeurs de propriété OSGi liées à la configuration OSGi principale

## Modèles Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

La console Sling Models se trouve à l’adresse suivante :

+ Outils > Opérations > Console Web > État > Modèles Sling
+ Ou directement à : [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Aspects clés qui aident à exécuter les activités de débogage :

+ La validation des modèles Sling est enregistrée sur le type de ressource approprié.
+ La validation des modèles Sling est adaptable à partir des objets corrects (Resource ou SlingHttpRequestServlet).
+ La validation des exportateurs de modèles Sling est correctement enregistrée
