---
title: Déboguer à distance le SDK d’AEM
description: Le fichier quickstart local du SDK AEM permet le débogage Java à distance à partir de votre IDE, ce qui vous permet de parcourir l’exécution du code en direct dans AEM pour comprendre le flux d’exécution exact.
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 452
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 100%

---

# Déboguer à distance le SDK d’AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

Le fichier quickstart local du SDK AEM permet le débogage Java à distance à partir de votre IDE, ce qui vous permet de parcourir l’exécution du code en direct dans AEM pour comprendre le flux d’exécution exact.

Pour connecter un débogueur distant à AEM, le fichier quickstart local du SDK AEM doit être démarré avec des paramètres spécifiques (`-agentlib:...`) permettant à l’IDE de s’y connecter.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ Le SDK AEM ne prend en charge que Java 11.
+ `address` spécifie le port qu’AEM écoute pour les connexions de débogage à distance et peut être remplacé par n’importe quel port disponible sur l’ordinateur de développement local.
+ Le dernier paramètre (par ex., `aem-author-p4502.jar`) est le fichier Quickstart Jar du SDK AEM. Il peut s’agir du service de création AEM (`aem-author-p4502.jar`) ou celui de publication (`aem-publish-p4503.jar`).


## Instructions de configuration d’IDE

La plupart des IDE Java prennent en charge le débogage à distance des programmes Java, mais les étapes de configuration exactes de chaque IDE varient. Veuillez consulter les instructions de configuration de débogage à distance de votre IDE pour connaître les étapes exactes. En règle générale, les configurations d’IDE nécessitent que :

+ L’hôte que le fichier quickstart local du SDK d’AEM écoute, à savoir `localhost`.
+ Le port que le fichier quickstart local du SDK AEM écoute pour la connexion de débogage à distance, qui est le port spécifié par le paramètre `address` lors du lancement du fichier quickstart local du SDK AEM.
+ Parfois, le ou les projets Maven qui fournissent le code source au débogage à distance doivent être spécifiés ; il s’agit de vos projets Maven de lot OSGi.

### Instructions de configuration

+ [Configuration du débogueur distant Java VS Code](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuration du débogueur distant IntelliJ IDEA](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Configuration du débogueur distant Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
