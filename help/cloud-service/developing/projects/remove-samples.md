---
title: Suppression d’exemples d’un projet Maven AEM
description: Découvrez comment nettoyer et supprimer un exemple de code d’un projet AEM généré par AEM Project Archetype.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 10%

---


# Suppression d’exemples d’un projet Maven AEM

Découvrez comment nettoyer et supprimer l’exemple de code généré d’un projet AEM généré par AEM Project Archetype.

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## Ressources

+ [AEM Archétype de projet Maven](https://github.com/adobe/aem-project-archetype)

## Commandes

Les commandes suivantes peuvent être exécutées pour supprimer les fichiers d’exemple générés du projet Maven AEM :

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## Modifications

Supprimez le fichier `<div class="helloworld" ...></div>` du dossier:

```
ui.frontend/src/main/webpack/static/index.html
```

Supprimez le `<helloworld>` Définition d’instance de composant à partir de :

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
