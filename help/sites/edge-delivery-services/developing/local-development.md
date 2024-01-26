---
title: Configuration d’un environnement de développement local pour les Edge Delivery Services
description: Comment configurer un environnement de développement local pour les Edge Delivery Services.
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 169
exl-id: 0f3e50f0-88d8-46be-be8b-0f547c3633a6
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---

# Configuration d’un environnement de développement local

Comment configurer un environnement de développement local pour le développement pour les Edge Delivery Services.

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## Étapes décrites dans la vidéo

1. Installation de l’interface de ligne de commande AEM

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. Modifiez le répertoire dans votre répertoire de projet qui est un référentiel Git créé à partir de [AEM standard](https://github.com/adobe/aem-boilerplate) modèle.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. Exécutez l’interface de ligne de commande AEM pour démarrer l’instance AEM locale.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. Ouvrez http://localhost:3000/ votre navigateur web pour afficher votre site web AEM.
