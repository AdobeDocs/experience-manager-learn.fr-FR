---
title: Débogage à distance du SDK AEM
description: Le démarrage rapide local du SDK AEM permet le débogage Java à distance à partir de votre IDE, ce qui vous permet de parcourir l’exécution du code en direct dans AEM pour comprendre le flux d’exécution exact.
feature: Outils de développement
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Développement
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# Débogage à distance du SDK AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

Le démarrage rapide local du SDK AEM permet le débogage Java à distance à partir de votre IDE, ce qui vous permet de parcourir l’exécution du code en direct dans AEM pour comprendre le flux d’exécution exact.

Pour connecter un débogueur distant à AEM, le démarrage rapide local du SDK AEM doit être démarré avec des paramètres spécifiques (`-agentlib:...`) permettant à l’IDE de s’y connecter.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` indique que l’AEM du port est activé pour les connexions de débogage à distance et peut être remplacé par n’importe quel port disponible sur l’ordinateur de développement local.
+ Le dernier paramètre (par ex. `aem-author-p4502.jar`) est le fichier Jar de démarrage rapide du SDK AEM. Il peut s’agir du service AEM Author (`aem-author-p4502.jar`) ou du service AEM Publish (`aem-publish-p4503.jar`).

## Instructions de configuration IDE

La plupart des IDE Java prennent en charge le débogage à distance des programmes Java, mais les étapes de configuration exactes de chaque IDE varient. Consultez les instructions de configuration de débogage à distance de votre IDE pour connaître les étapes exactes. En règle générale, les configurations IDE nécessitent les éléments suivants :

+ Le démarrage rapide local du SDK d’AEM hôte écoute, qui est `localhost`.
+ Le démarrage rapide local du SDK AEM port écoute la connexion de débogage à distance, qui est le port spécifié par le paramètre `address` lors du démarrage AEM démarrage rapide local du SDK.
+ Parfois, le ou les projets Maven qui fournissent le code source au débogage distant doivent être spécifiés ; il s’agit de vos projets Maven de bundle OSGi.

### Configuration des instructions

+ [Débogueur à distance Java VS Code](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuration du débogueur distant IntelliJ IDEA](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Configuration du débogueur distant Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
