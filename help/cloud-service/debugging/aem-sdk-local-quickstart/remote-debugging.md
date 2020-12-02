---
title: Débogage à distance du SDK AEM
description: Le démarrage rapide local du SDK AEM permet le débogage Java à distance à partir de votre IDE, ce qui vous permet de passer en revue l’exécution du code en direct dans AEM pour comprendre le flux d’exécution exact.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Débogage à distance du SDK AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

Le démarrage rapide local du SDK AEM permet le débogage Java à distance à partir de votre IDE, ce qui vous permet de passer en revue l’exécution du code en direct dans AEM pour comprendre le flux d’exécution exact.

Pour connecter un débogueur distant à AEM, le démarrage rapide local du SDK AEM doit être démarré avec des paramètres spécifiques (`-agentlib:...`) permettant à l&#39;IDE de se connecter à celui-ci.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` spécifie le port AEM écoute les connexions de débogage à distance et peut être modifié en n&#39;importe quel port disponible sur l&#39;ordinateur de développement local.
+ Le dernier paramètre (ex. `aem-author-p4502.jar`) est l&#39;AEM SKD Quickstart Jar. Il peut s’agir du service Auteur AEM (`aem-author-p4502.jar`) ou du service Publier AEM (`aem-publish-p4503.jar`).

## Instructions de configuration IDE

La plupart des IDE Java prennent en charge le débogage à distance des programmes Java, mais les étapes de configuration exactes de chaque IDE varient. Consultez les instructions de configuration du débogage à distance de votre IDE pour connaître les étapes exactes. En règle générale, les configurations IDE nécessitent :

+ Le démarrage rapide local du SDK AEM hôte est à l’écoute, c’est-à-dire `localhost`.
+ Le démarrage rapide local du kit de développement AEM port écoute la connexion de débogage à distance, qui est le port spécifié par le paramètre `address` lors du démarrage rapide local du kit SDK AEM.
+ Occasionnellement, les projets Maven qui fournissent le code source au débogage distant doivent être spécifiés ; il s’agit de votre ou vos projets d’expert OSGi.

### Configuration des instructions

+ [Débogueur distant Java Code VS configuré](https://code.visualstudio.com/docs/java/java-debugging)
+ [Débogueur distant IntelliJ IDEA configuré](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Débogueur Eclipse Remote configuré](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
