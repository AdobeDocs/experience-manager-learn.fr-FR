---
title: Prendre en main l’éditeur de SPA et de SPA distante - Présentation
description: Bienvenue dans ce tutoriel en plusieurs parties destiné aux développeurs et développeuses qui cherchent à améliorer une SPA distante existante avec du contenu AEM modifiable en utilisant l’éditeur de SPA d’AEM.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: 0fff8b53e3dffb835e070444b55a72f0b0cc3d14
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 98%

---

# Présentation

Bienvenue dans ce tutoriel en plusieurs parties destiné aux développeurs et développeuses qui cherchent à améliorer une SPA distante existante basée sur React (ou Next.js) avec du contenu AEM modifiable en utilisant l’éditeur de SPA d’AEM.

Ce tutoriel s’appuie sur l’[application GraphQL WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr), une application React qui consomme le contenu AEM par fragments de contenu via les API GraphQL d’AEM, mais qui ne fournit pas de créationcontextuelle du contenu de la SPA.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## À propos du tutoriel

Ce tutoriel vise à illustrer comment une SPA distante, ou une SPA fonctionnant en dehors du contexte AEM, peut être mise à jour pour consommer et diffuser du contenu créé dans AEM.

La plupart des activités du tutoriel se concentrent sur le développement de JavaScript, toutefois, des aspects critiques concernant AEM sont couverts. Ces aspects comprennent la définition de l’endroit où le contenu est créé et stocké dans AEM et le mappage des itinéraires SPA aux pages AEM.

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et est composé de deux projets :

1. Le __projet AEM__ contient la configuration et le contenu qui doivent être déployés vers AEM.
1. Le projet __application WKND__ est la SPA à intégrer avec l’éditeur de SPA d’AEM.

## Dernier code

+ Le point de départ du code de ce tutoriel se trouve sur [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) dans le dossier `remote-spa-tutorial`.

## Prérequis

Ce tutoriel nécessite les éléments suivants :

+ [SDK AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr)
+ [Node.js v18](https://nodejs.org/fr/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6 et ultérieure](https://maven.apache.org/)
+ [Git.](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip ou ultérieure.](https://github.com/adobe/aem-guides-wknd/releases)
+ [Code source aem-guides-wknd-graphql.](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Ce tutoriel fonctionne avec les éléments suivants :

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) comme IDE
+ Un répertoire de travail de `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Exécution du SDK d’AEM en tant que service de création sur `http://localhost:4502`
+ Exécution du SDKd’ AEM avec le compte `admin` local et le mot de passe `admin`
+ L’exécution de la SPA sur `http://localhost:3000`.

>[!NOTE]
>
> **Vous avez besoin d’aide pour configurer votre environnement de développement local ?** Consultez le [guide suivant pour configurer un environnement de développement local à l’aide du SDK d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).

## 1. Configurer AEM pour l’éditeur de SPA

Des configurations AEM sont nécessaires pour intégrer le SPA avec l’éditeur de SPA d’AEM. Ces configurations sont gérées et déployées via un projet AEM. Dans ce chapitre, vous apprendrez quelles configurations sont nécessaires et comment les définir.

+ [Apprenez à configurer AEM pour l’éditeur de SPA.](./aem-configure.md)

## 2. Amorcer la SPA

Pour que l’éditeur de SPA d’AEM puisse intégrer une SPA dans son contexte de création, quelques ajouts doivent être effectués sur la SPA.

+ [Apprenez comment amorcer la SPA pour l’éditeur de SPA d’AEM.](./spa-bootstrap.md)

## 3. Composants fixes modifiables

Tout d’abord, envisagez d’ajouter un « composant fixe » modifiable à la SPA. Ceci illustre comment un développeur ou une développeuse peut placer un composant modifiable spécifique dans la SPA. L’auteur ou l’autrice peut modifier le contenu du composant, il ou elle ne peut cependant ni le supprimer ni en modifier l’emplacement, la position ou la taille.

+ [En savoir plus sur les composants fixes modifiables.](./spa-fixed-component.md)

## 4. Composants de conteneur modifiables

Ensuite, étudiez l’ajout d&#39;un « composant de conteneur » modifiable à la SPA. Ceci illustre la manière dont un développeur ou une développeuse peut placer un composant de type conteneur dans la SPA. Les composants de type conteneur permettent aux auteurs et autrices d’y placer le composant autorisé et d’ajuster la disposition des composants.

+ [En savoir plus sur les composants de type conteneur modifiables.](./spa-container-component.md)

## 5. Itinéraires dynamiques et composants modifiables

Enfin, utilisez les concepts présentés dans les chapitres précédents pour créer des itinéraires dynamiques ; ces itinéraires affichent un contenu différent en fonction du paramètre de l’itinéraire. Ceci illustre la manière dont l’éditeur de SPA d’AEM peut être utilisé pour créer du contenu sur des itinéraires pilotés et dérivés par programmation.

+ [En savoir plus sur les itinéraires dynamiques et les composants modifiables.](./spa-dynamic-routes.md)

## Ressources supplémentaires

+ [Composants React modifiables de SPA d’AEM.](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
