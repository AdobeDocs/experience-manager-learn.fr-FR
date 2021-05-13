---
title: Prise en main de SPA Editor et de Remote SPA - Présentation
description: Bienvenue dans le tutoriel en plusieurs parties pour les développeurs qui cherchent à enrichir un SPA distant existant avec un contenu AEM modifiable à l'aide de l'éditeur de .
topic: Sans tête, SPA, développement
feature: SPA Éditeur, Composants principaux, API, Développement
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 6%

---


# Présentation

Bienvenue dans le tutoriel en plusieurs parties pour les développeurs qui cherchent à développer un SPA distant existant basé sur React (ou Next.js) avec un contenu AEM modifiable à l’aide de l’éditeur de .

Ce didacticiel s’appuie sur l’[application WKND GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr), une application React qui utilise AEM contenu Fragment de contenu par le biais des API AEM GraphQL, mais qui ne fournit aucune création de contenu en contexte de contenu SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## À propos du didacticiel

Ce didacticiel a pour but d’illustrer comment un SPA distant, ou un SPA s’exécutant en dehors du contexte de l’AEM, peut être mis à jour pour consommer et diffuser du contenu créé dans l’.

La plupart des activités du didacticiel se concentrent sur le développement de JavaScript, mais les aspects critiques sont couverts et tournent autour des AEM. Ces aspects comprennent la définition de l’emplacement de création et de stockage du contenu dans AEM et le mappage SPA itinéraires vers les pages de l’AEM.

Le didacticiel est conçu pour fonctionner avec **AEM en tant que Cloud Service** et se compose de deux projets :

1. Le __AEM projet__ contient la configuration et le contenu qui doivent être déployés sur AEM.
1. __WKND__ Appproject est le SPA à intégrer à AEM SPA rédacteur en chef

## Dernier code

+ Le code de ce didacticiel est disponible sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) dans la branche `feature/spa-editor`.

## Prérequis

Ce didacticiel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip ou version ultérieure](https://github.com/adobe/aem-guides-wknd/releases)
+ [code source aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)

Ce didacticiel suppose :

+ [Microsoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas the IDE
+ Un répertoire de travail de `~/Code/wknd-app`
+ Exécution du SDK AEM en tant que service Auteur sur `http://localhost:4502`
+ Exécution du SDK AEM avec le compte `admin` local avec le mot de passe `admin`
+ Exécution du SPA le `http://localhost:3000`

>[!NOTE]
>
> **Vous avez besoin d&#39;aide pour configurer votre environnement de développement local ?** Consultez le guide  [suivant pour configurer un environnement de développement local à l’aide de l’AEM en tant que SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) Cloud Service.


## Configuration rapide

La configuration rapide vous permet de vous familiariser avec les SPA d’application WKND et AEM Éditeur de SPA en 15 minutes. Cette configuration accélérée vous conduit directement à l&#39;état final du didacticiel, ce qui vous permet d&#39;explorer la création du SPA dans AEM Éditeur de SPA.

+ [En savoir plus sur la configuration rapide](./quick-setup.md)

## 1. Configuration de AEM pour SPA Editor

Des configurations AEM sont requises pour intégrer le SPA à l’éditeur d’. Ces configurations sont gérées et déployées via un projet AEM. Dans ce chapitre, découvrez les configurations nécessaires et comment les définir.

+ [Découvrez comment configurer AEM pour SPA Editor](./aem-configure.md)

## 2. Bootstrap de la SPA

Pour que AEM SPA Editor intègre un SPA dans son contexte de création, quelques ajouts doivent être apportés au .

+ [Découvrez comment amorcer le SPA pour AEM Éditeur SPA](./spa-bootstrap.md)

## 3. Composants fixes modifiables

Tout d’abord, explorez l’ajout d’un &quot;composant fixe&quot; modifiable au SPA. Ceci illustre comment un développeur peut placer un composant modifiable spécifique dans le SPA. Bien que l’auteur puisse modifier le contenu du composant, il ne peut pas le supprimer ni modifier son emplacement, sa position ou sa taille.

+ [En savoir plus sur les composants fixes modifiables](./spa-fixed-component.md)

## 4. Composants de conteneur modifiables

Ensuite, explorez l’ajout d’un &quot;composant de conteneur&quot; modifiable au SPA. Ceci illustre comment un développeur peut placer un composant de conteneur dans le SPA. Les composants de conteneur permettent aux auteurs d’y placer des composants autorisés et d’ajuster la disposition des composants.

+ [En savoir plus sur les composants de conteneur modifiables](./spa-container-component.md)

## 5. Routes dynamiques et composants modifiables

Enfin, utiliser les concepts décrits dans les chapitres précédents pour les itinéraires dynamiques ; routes qui affichent un contenu différent en fonction du paramètre de la route. Cela illustre comment AEM SPA Editor peut être utilisé pour créer du contenu sur des itinéraires qui sont pilotés et dérivés par programmation.

+ [En savoir plus sur les itinéraires dynamiques et les composants modifiables](./spa-dynamic-routes.md)

## Ressources supplémentaires

+ [Modification d&#39;un SPA externe dans AEM Docs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [Composants WCM AEM - Réagir à l’implémentation de base](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [Composants WCM AEM - Éditeur Spa - Réagir à l’implémentation de base](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
