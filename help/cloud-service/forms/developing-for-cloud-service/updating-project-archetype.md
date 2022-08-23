---
title: Mise à jour du projet de service cloud avec l’archétype le plus récent
description: Mise à jour du projet de service cloud AEM Forms avec l’archétype le plus récent
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 1%

---

# Migration à partir de l’ancien archétype aem

Pour mettre à jour votre projet AEM Forms existant avec l’archétype maven le plus récent, vous devez copier manuellement votre code/configuration, etc., de l’ancien projet au nouveau projet.

Les étapes suivantes ont été suivies pour migrer le projet créé à l’aide de l’archetype 30 vers le projet archetype 33.

## Créez un projet Maven à l’aide de l’archétype le plus récent.

* Ouvrez une invite de commande et accédez à c:\cloudmanager
* Créez un projet Maven à l’aide de l’archétype le plus récent.
* Copiez et collez le contenu de la [fichier texte](assets/creating-maven-project.txt) dans la fenêtre d’invite de commande. Vous devrez peut-être modifier DarchetypeVersion=33 en fonction de la variable [dernière version](https://github.com/adobe/aem-project-archetype/releases). Archetype 33 comprend de nouveaux thèmes AEM Forms.
Puisque nous créons le nouveau projet Maven dans le dossier cloudmanager qui a déjà un projet aem-banking-application, vous devez modifier la variable **DartifactId** de aem-banking-application à quelque chose de différent. J’ai utilisé aem-banking-application1 pour cet article.

>[!NOTE]
>
>Si vous déployez ce nouveau projet comme l’est l’instance de service cloud, HandleFormSubmission et SubmitToAEMServlet ne seront pas inclus. En effet, chaque fois que vous déployez un projet à l’aide de cloud manager, tout élément situé sous le dossier des applications est supprimé et remplacé.

## Copiez votre code Java

Une fois votre projet créé, vous pouvez commencer à copier du code/des configurations, etc., de l’ancien projet à ce nouveau projet.

* Copiez le servlet HandleFormSubmission de ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
to

   ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiez CustomSubmit de
   ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` de aem-banking-application au projet aem-banking-application1

* importez le nouveau projet dans IntelliJ

* Mettez à jour le fichier filter.xml dans le module ui.apps du projet aem-banking-application1 pour inclure la ligne suivante :
   ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Une fois que vous avez copié tout le code dans votre nouveau projet, vous pouvez transférer ce projet vers le gestionnaire de cloud.

>[!NOTE]
>
>Pour synchroniser le contenu (Forms adaptatif, modèle de données de formulaire, etc.) dans votre nouveau projet, vous devez créer la structure de dossiers appropriée dans votre projet IntelliJ, puis synchroniser votre projet IntelliJ avec votre instance AEM à l’aide de la commande Get de l’outil de référentiel.
