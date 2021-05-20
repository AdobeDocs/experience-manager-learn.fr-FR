---
title: Débogage AEM SDK à l’aide de la console web OSGi
description: Le démarrage rapide local du SDK d’AEM dispose d’une console web OSGi qui fournit diverses informations et introspections dans le runtime AEM local. Ces informations sont utiles pour comprendre comment votre application est reconnue par les fonctions et d’ d’.
feature: Outils de développement
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Développement
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 5%

---


# Débogage AEM SDK à l’aide de la console web OSGi

Le démarrage rapide local du SDK d’AEM dispose d’une console web OSGi qui fournit diverses informations et introspections dans le runtime AEM local. Ces informations sont utiles pour comprendre comment votre application est reconnue par les fonctions et d’ d’.

AEM fournit de nombreuses consoles OSGi, chacune fournissant des informations clés sur différents aspects d’AEM. Toutefois, les éléments suivants sont généralement les plus utiles pour déboguer votre application.

## Lots

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

La console Bundles est un catalogue des lots OSGi, ainsi que leurs détails, déployés sur AEM, avec la possibilité de les démarrer et de les arrêter.

La console Lots se trouve à l’adresse :

+ Outils > Opérations > Console web > OSGi > Lots
+ Ou directement à l’adresse : [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Cliquer sur chaque lot fournit des détails qui aident à déboguer votre application.

+ La validation du lot OSGi est présente
+ Validation si un lot OSGi est principal
+ Déterminer si un lot OSGi comporte des imports insatisfaits qui l’empêchent de démarrer

## Composants

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

La console Composants est un catalogue de tous les composants OSGi déployés sur AEM. Elle fournit toutes les informations les concernant, depuis leur cycle de vie de composant OSGi défini jusqu’aux services OSGi auxquels ils peuvent faire référence.

La console Composants se trouve à l’adresse suivante :

+ Outils > Opérations > Console web > OSGi > Composants
+ Ou directement à l’adresse : [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Principaux aspects des activités de débogage :

+ La validation du lot OSGi est présente
+ Validation si un lot OSGi est principal
+ Déterminer si un lot OSGi comporte des imports insatisfaits qui l’empêchent de démarrer
+ Obtention du PID du composant, afin de créer des configurations OSGi pour lui dans Git
+ Identification des valeurs de propriété OSGi liées à la configuration OSGi principale

## Modèles Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

La console Modèles Sling se trouve à l’adresse :

+ Outils > Opérations > Console web > État > Modèles Sling
+ Ou directement à l’adresse : [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Principaux aspects des activités de débogage :

+ La validation des modèles Sling est enregistrée dans le type de ressource approprié.
+ La validation des modèles Sling est adaptable à partir des objets corrects (Resource ou SlingHttpRequestServlet).
+ La validation des exportateurs de modèles Sling est correctement enregistrée
