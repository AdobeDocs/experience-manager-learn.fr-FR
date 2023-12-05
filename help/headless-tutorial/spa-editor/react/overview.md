---
title: Prise en main de l’Éditeur de SPA d’AEM et de React
description: Créez votre première application monopage React modifiable dans Adobe Experience Manager (AEM) avec l’application monopage WKND. Découvrez comment créer une SPA à l’aide du framework React JS avec lʼéditeur de SPA dʼAEM. Ce tutoriel en plusieurs parties décrit lʼimplémentation d’une application React pour une marque de style de vie fictive, WKND. Le tutoriel couvre la création de bout en bout de la SPA et lʼintégration à AEM.
version: Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 100
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 100%

---

# Créer votre première SPA React dans AEM {#overview}

{{edge-delivery-services}}

Bienvenue dans un tutoriel en plusieurs parties conçu pour les développeurs et développeuses qui découvrent l’**éditeur de SPA** dans Adobe Experience Manager (AEM). Ce tutoriel présente l’implémentation d’une application React pour une marque fictive de style de vie, WKND. L’application React est développée et conçue pour être déployée avec l’éditeur de SPA d’AEM, qui mappe les composants React aux composants d’AEM. Les SPA terminées, déployées sur AEM, peuvent être créées dynamiquement à l’aide des outils de modification en ligne traditionnels d’AEM.

![SPA finale implémentée.](assets/wknd-spa-implementation.png)

*Implémentation de la SPA WKND*

## À propos

Le tutoriel est conçu pour fonctionner avec **AEM as a Cloud Service** et est rétrocompatible avec **AEM 6.5.4+** et **AEM 6.4.8+**.

## Dernier code

Vous trouverez tout le code du tutoriel sur [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

La [base de code la plus récente](https://github.com/adobe/aem-guides-wknd-spa/releases) est disponible sous forme de packages AEM téléchargeables.

## Prérequis

Avant de commencer ce tutoriel, il vous faut :

* Des connaissances de base en HTML, CSS et JavaScript
* Des connaissance de base de [React](https://reactjs.org/tutorial/tutorial.html)

*Bien que cela ne soit pas obligatoire, il est préférable de comprendre les bases du [développement des composants traditionnels d’AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr).*

## Environnement de développement local {#local-dev-environment}

Pour suivre ce tutoriel, un environnement de développement local est nécessaire. Les captures d’écran et les vidéos sont enregistrées à l’aide du SDK AEM as a Cloud Service exécuté dans un environnement macOS avec [Visual Studio Code](https://code.visualstudio.com/) comme IDE. Sauf indication contraire, les commandes et le code doivent être indépendants du système d’exploitation local.

### Logiciels requis

* [SDK d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=fr), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=fr#aem-65) ou [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=fr#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 ou version ultérieure)
* [Node.js](https://nodejs.org/en/) et [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Vous découvrez AEM as a Cloud Service ?** Consultez le [guide suivant pour configurer un environnement de développement local à l’aide du SDK d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr).
>
> **Vous découvrez AEM 6.5 ?** Consultez le [guide suivant pour configurer un environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=fr).

## Étapes suivantes {#next-steps}

Qu’attendez-vous ? Démarrez le tutoriel en accédant au chapitre [Créer un projet](create-project.md) et découvrez comment générer un projet activé pour l’éditeur de SPA à l’aide de l’archétype de projet d’AEM.
