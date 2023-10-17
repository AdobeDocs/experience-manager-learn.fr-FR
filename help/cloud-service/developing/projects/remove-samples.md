---
title: Supprimer les exemples d’un projet AEM Maven
description: Découvrez comment nettoyer et supprimer un exemple de code d’un projet AEM généré par l’archétype de projet AEM.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '88'
ht-degree: 100%

---

# Supprimer les exemples d’un projet AEM Maven

Découvrez comment nettoyer et supprimer l’exemple de code d’un projet AEM généré par l’archétype de projet AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Ressources

+ [Archétype de projet AEM Maven](https://github.com/adobe/aem-project-archetype)

## Commandes

Les commandes suivantes peuvent être exécutées pour supprimer les fichiers d’exemple générés du projet AEM Maven :

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

Supprimez le fichier `<div class="helloworld" ...></div>` du dossier :

```
ui.frontend/src/main/webpack/static/index.html
```

Supprimez la définition d’instance de composant `<helloworld>` à partir de :

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
