---
title: Déboguer le SDK AEM à l’aide de la console web OSGi
description: Le démarrage rapide local du SDK d’AEM dispose d’une console web OSGi qui fournit diverses informations et introspections dans l’exécution locale d’AEM qui sont utiles pour comprendre comment votre application est reconnue par AEM et comment elle fonctionne en son sein.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '393'
ht-degree: 100%

---

# Déboguer le SDK AEM à l’aide de la console web OSGi

Le démarrage rapide local du SDK d’AEM dispose d’une console web OSGi qui fournit diverses informations et introspections dans l’exécution locale d’AEM qui sont utiles pour comprendre comment votre application est reconnue par AEM et comment elle fonctionne en son sein.

AEM fournit de nombreuses consoles OSGi, chacune fournissant des informations clés sur différents aspects d’AEM. Toutefois, les éléments suivants sont généralement les plus utiles pour déboguer votre application.

## Lots

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

La console Lots est un catalogue des lots OSGi déployés sur AEM, et de leurs détails, avec la possibilité ad hoc de les démarrer et de les arrêter.

La console Lots se trouve à l’emplacement suivant :

+ Outils > Opérations > Console web > OSGi > Lots.
+ Ou directement à l’adresse : [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Cliquer sur chaque lot fournit des détails qui aident à déboguer votre application.

+ Valider si un lot OSGi est présent
+ Valider si un lot OSGi est actif
+ Déterminer si un lot OSGi comporte des import non satisfaites qui l’empêchent de démarrer

## Composants

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

La console Composants est un catalogue de tous les composants OSGi déployés sur AEM. Elle fournit toutes les informations les concernant, depuis leur cycle de vie de composant OSGi défini jusqu’aux services OSGi auxquels ils peuvent faire référence.

La console Composants se trouve à l’emplacement suivant :

+ Outils > Opérations > Console web > OSGi > Composants.
+ Ou directement à l’adresse : [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Principaux aspects qui facilitent les activités de débogage :

+ Valider si un lot OSGi est présent
+ Valider si un lot OSGi est actif
+ Déterminer si un lot OSGi comporte des import non satisfaites qui l’empêchent de démarrer
+ Obtenir le PID du composant afin de créer des configurations OSGi correspondantes dans Git.
+ Identifier des valeurs de propriété OSGi liées à la configuration OSGi active.

## Modèles Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

La console Modèles Sling se trouve à l’emplacement suivant :

+ Outils > Opérations > Console web > Statut > Modèles Sling.
+ Ou directement à l’adresse : [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Principaux aspects qui facilitent les activités de débogage :

+ Valider si les modèles Sling sont enregistrés dans le type de ressource approprié
+ Valider si les modèles Sling sont adaptables à partir des objets corrects (Resource ou SlingHttpRequestServlet)
+ Valider si les exporteurs de modèles Sling sont correctement enregistrés
