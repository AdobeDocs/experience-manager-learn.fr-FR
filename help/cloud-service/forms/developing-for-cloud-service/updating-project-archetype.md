---
title: Mettre à jour le projet de service cloud avec l’archétype le plus récent
description: Mettre à jour le projet de service cloud AEM Forms avec l’archétype le plus récent
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: AEM Project Archetype
jira: KT-9534
exl-id: c2cd9c52-6f00-4cfe-a972-665093990e5d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 100%

---

# Migrer à partir de l’ancien archétype AEM

Pour mettre à jour votre projet AEM Forms existant avec l’archétype maven le plus récent, vous devez copier manuellement votre code, votre configuration, etc., de l’ancien projet vers le nouveau projet.

Les étapes suivantes ont permis de migrer le projet créé à l’aide de l’archetype 30 vers l’archetype 33.

## Créer un projet maven à l’aide de l’archétype le plus récent

* Ouvrez l’invite de commande et accédez à c:\cloudmanager
* Créez un projet maven à l’aide de l’archétype le plus récent.
* Copiez et collez le contenu du [fichier texte](assets/creating-maven-project.txt) dans la fenêtre d’invite de commande. Vous devrez peut-être modifier DarchetypeVersion=33 en fonction de la [dernière version](https://github.com/adobe/aem-project-archetype/releases). L’archetype 33 comprend de nouveaux thèmes AEM Forms.
Puisque nous créons le nouveau projet maven dans le dossier cloudmanager qui contient déjà un projet aem-banking-application, vous devez modifier **DartifactId** et choisir une variable différente pour aem-banking-application. J’ai utilisé aem-banking-application1 pour cet article.

>[!NOTE]
>
>Si vous déployez ce nouveau projet tel quel, l’instance de service cloud ne contiendra pas HandleFormSubmission ni SubmitToAEMServlet. En effet, chaque fois que vous déployez un projet à l’aide de Cloud Manager, toutes les données se trouvant sous le dossier `/apps` sont supprimées et remplacées.

## Copier votre code Java

Une fois votre projet créé, vous pouvez commencer à copier du code, des configurations, etc., de l’ancien projet vers ce nouveau projet.

* Copiez le servlet HandleFormSubmission de ```C:\CloudManager\aem-banking-application\core\src\main\java\com\aem\bankingapplication\core\servlets```
vers
  ```C:\CloudManager\aem-banking-application1\core\src\main\java\com\aem\bankingapplication\core\servlets```

* Copiez CustomSubmit de
  ```C:\CloudManager\aem-banking-application\ui.apps\src\main\content\jcr_root\apps\bankingapplication\SubmitToAEMServlet``` depuis aem-banking-application vers le projet aem-banking-application1.

* Importez le nouveau projet maven vers IntelliJ

* Mettez à jour le fichier filter.xml dans le module ui.apps du projet aem-banking-application1 pour inclure la ligne suivante :
  ```<filter root="/apps/bankingapplication/SubmitToAEMServlet"/>```

Une fois que vous avez copié tout le code dans votre nouveau projet, vous pouvez transférer ce projet vers Cloud Manager.

>[!NOTE]
>
>Pour synchroniser le contenu (formulaires adaptatifs, modèle de données de formulaire, etc.) dans votre nouveau projet, vous devez créer la structure de dossiers appropriée dans votre projet IntelliJ, puis synchroniser votre projet IntelliJ avec votre instance AEM à l’aide de la commande GET de l’outil de référentiel.
