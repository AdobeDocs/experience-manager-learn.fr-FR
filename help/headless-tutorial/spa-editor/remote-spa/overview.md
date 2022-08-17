---
title: Prise en main de SPA Editor et de Remote SPA - Présentation
description: Bienvenue dans le tutoriel en plusieurs parties pour les développeurs qui souhaitent enrichir une SPA distante existante avec du contenu AEM modifiable à l’aide de l’éditeur d’.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 8%

---

# Présentation

Bienvenue dans le tutoriel en plusieurs parties pour les développeurs qui souhaitent enrichir un SPA distant basé sur React (ou Next.js) existant avec du contenu AEM modifiable à l’aide d’Editor.

Ce tutoriel s’appuie sur la méthode [Application WKND GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr), une application React qui consomme AEM contenu de fragment de contenu sur AEM API GraphQL, mais ne fournit aucune création de contenu en contexte de contenu SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## À propos du didacticiel

Le tutoriel a pour but d’illustrer comment une SPA distante, ou un SPA s’exécutant en dehors du contexte d’une activité d’, peut être mise à jour pour consommer et diffuser du contenu créé dans.

La plupart des activités du tutoriel se concentrent sur le développement de JavaScript, mais les aspects critiques sont abordés autour des AEM. Ces aspects incluent la définition de l’emplacement de création et de stockage du contenu dans AEM et le mappage SPA itinéraires vers les pages d’AEM.

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et se compose de deux projets :

1. Le __AEM projet__ contient la configuration et le contenu qui doivent être déployés sur AEM.
1. __Application WKND__ Le projet est le SPA à intégrer à AEM Éditeur de la bibliothèque d’SPA

## Dernier code

+ Le code de ce tutoriel se trouve sur la page [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) sur le `feature/spa-editor` branche.

## Conditions préalables

Ce tutoriel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip ou version ultérieure](https://github.com/adobe/aem-guides-wknd/releases)
+ [code source aem-guides-wknd-graphql (branche) : feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)

Ce tutoriel suppose :

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) comme IDE
+ Un répertoire de travail de `~/Code/wknd-app`
+ Exécution du SDK AEM en tant que service d’auteur sur `http://localhost:4502`
+ Exécution du SDK AEM avec le fichier local `admin` compte avec mot de passe `admin`
+ Exécution du SPA sur `http://localhost:3000`

>[!NOTE]
>
> **Vous avez besoin d’aide pour configurer votre environnement de développement local ?** Consultez la section [le guide suivant pour configurer un environnement de développement local à l’aide du SDK as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).


## Configuration rapide

La configuration rapide vous permet d’utiliser l’SPA d’application WKND et AEM Éditeur d’SPA en 15 minutes. Cette configuration accélérée vous permet d’accéder directement à l’état final du tutoriel, ce qui vous permet d’explorer la création de la SPA dans AEM Éditeur.

+ [En savoir plus sur la configuration rapide](./quick-setup.md)

## 1. Configuration d’AEM pour SPA Editor

Des configurations d’AEM sont requises pour intégrer le SPA à l’éditeur d’. Ces configurations sont gérées et déployées via un projet AEM. Dans ce chapitre, découvrez les configurations nécessaires et comment les définir.

+ [Découvrez comment configurer AEM pour SPA Editor](./aem-configure.md)

## 2. Bootstrap de la SPA

Pour qu’AEM SPA Éditeur intègre un SPA dans son contexte de création, quelques ajouts doivent être apportés au .

+ [Découvrez comment démarrer le SPA pour AEM Éditeur de SPA](./spa-bootstrap.md)

## 3. Composants fixes modifiables

Tout d’abord, explorez l’ajout d’un &quot;composant fixe&quot; modifiable au SPA. Cela illustre la manière dont un développeur peut placer un composant modifiable spécifique, dans le SPA. Bien que l’auteur puisse modifier le contenu du composant, il ne peut pas le supprimer ni modifier son emplacement, son positionnement ou sa taille.

+ [En savoir plus sur les composants fixes modifiables](./spa-fixed-component.md)

## 4. Composants de conteneur modifiables

Ensuite, explorez l’ajout d’un &quot;composant de conteneur&quot; modifiable au SPA. Cela illustre la manière dont un développeur peut placer un composant de conteneur dans le SPA. Les composants de conteneur permettent aux auteurs d’y placer le composant autorisé et d’ajuster la mise en page des composants.

+ [En savoir plus sur les composants de conteneur modifiables](./spa-container-component.md)

## 5. Itinéraires dynamiques et composants modifiables

Enfin, utilisez les concepts présentés dans les chapitres précédents pour créer des itinéraires dynamiques. itinéraires qui affichent un contenu différent en fonction du paramètre de l’itinéraire. Cela illustre la manière dont l’éditeur SPA d’AEM peut être utilisé pour créer du contenu sur des itinéraires pilotés et dérivés par programmation.

+ [En savoir plus sur les itinéraires dynamiques et les composants modifiables](./spa-dynamic-routes.md)

## Ressources supplémentaires

+ [Modification d’un SPA externe dans AEM documents](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM Composants WCM - Mise en oeuvre principale React](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM Composants WCM - Éditeur de Spa - Mise en oeuvre principale React](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
