---
title: Prise en main de l’Éditeur AEM SPA et de React
description: Créez votre première application sur une seule page (SPA) React modifiable dans Adobe Experience Manager (AEM) avec la SPA WKND. Découvrez comment créer une SPA à l’aide du framework React JS avec lʼéditeur de SPA dʼAEM. Ce tutoriel en plusieurs parties décrit lʼimplémentation d’une application React pour une marque de style de vie fictive, WKND. Le tutoriel couvre la création de bout en bout de la SPA et lʼintégration à AEM.
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 33%

---

# Créer votre première SPA React dans AEM {#overview}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs qui découvrent le **Éditeur de SPA** dans Adobe Experience Manager (AEM). Ce tutoriel décrit la mise en oeuvre d’une application React pour une marque de style de vie fictive, WKND. L’application React est développée et conçue pour être déployée avec AEM SPA Editor, qui mappe les composants React aux composants d’AEM. Les SPA terminées, déployées sur AEM, peuvent être créées dynamiquement à l’aide des outils de modification en ligne traditionnels d’.

![SPA finale implémentée](assets/wknd-spa-implementation.png)

*Implémentation SPA WKND*

## À propos

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et est rétrocompatible avec **AEM 6.5.4+** et **AEM 6.4.8+**.

## Dernier code

Vous trouverez tout le code du tutoriel sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Le [base de code la plus récente](https://github.com/adobe/aem-guides-wknd-spa/releases) est disponible sous forme de packages AEM téléchargeables.

## Prérequis

Avant de commencer ce tutoriel, vous aurez besoin des éléments suivants :

* Connaissances de base en HTML, CSS et JavaScript
* Familiarité de base avec [React](https://reactjs.org/tutorial/tutorial.html)

*Bien qu’il ne soit pas nécessaire, il est préférable de posséder une compréhension élémentaire de la fonction [développement des composants AEM Sites traditionnels](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr).*

## Environnement de développement local {#local-dev-environment}

Un environnement de développement local est nécessaire pour terminer ce tutoriel. Les captures d’écran et les vidéos sont capturées à l’aide AEM SDK as a Cloud Service s’exécutant dans un environnement Mac OS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Les commandes et le code doivent être indépendants du système d’exploitation local, sauf indication contraire.

### Logiciels requis

* [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) ou [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/) et [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez la section [le guide suivant pour configurer un environnement de développement local à l’aide du SDK as a Cloud Service AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).
>
> **Vous découvrez AEM 6.5 ?** Consultez la section [guide de configuration d’un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## Étapes suivantes {#next-steps}

Qu&#39;attendez-vous?! Démarrez le tutoriel en accédant à la [Créer un projet](create-project.md) chapitres et découvrez comment générer un projet activé pour SPA Editor à l’aide d’AEM Project Archetype.
