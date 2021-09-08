---
title: Environnement de développement local pour AEM en tant que Cloud Service
description: Présentation de l’environnement de développement local d’Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 2%

---


# Configuration de l’environnement de développement local

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Présentation"
>abstract="La configuration de l’environnement de développement local pour AEM as a Cloud Service inclut les outils de développement nécessaires au développement, à la création et à la compilation de projets AEM, ainsi que les heures d’exécution locales, ce qui permet aux développeurs de valider rapidement les nouvelles fonctionnalités localement avant de les déployer vers en tant que Cloud Service via Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=fr" text="Conseils de développement"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="Principes de développement"

Ce tutoriel décrit la configuration d’un environnement de développement local pour Adobe Experience Manager (AEM) à l’aide du SDK AEM as a Cloud Service. Les outils de développement requis pour développer, créer et compiler des projets AEM, ainsi que les heures d’exécution locales, permettent aux développeurs de valider rapidement les nouvelles fonctionnalités localement avant de les déployer dans AEM as a Cloud Service via Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM en tant que pétale technologique de l&#39;environnement de développement local Cloud Service](./assets/overview/aem-sdk-technology-stack.png)

L’environnement de développement local pour AEM peut être divisé en trois groupes logiques :

+ Le __projet AEM__ contient le code, la configuration et le contenu personnalisés qui est l’application AEM personnalisée.
+ __Runtime d’AEM local__ qui exécute localement une version locale des services AEM Author et Publish.
+ __Exécution locale de Dispatcher__ qui exécute une version locale du serveur web Apache HTTP et de Dispatcher.

Ce tutoriel explique comment installer et configurer les éléments mis en surbrillance dans le diagramme ci-dessus, fournissant ainsi un environnement de développement local stable pour le développement AEM.

## Organisation du système de fichiers

Ce tutoriel a établi l’emplacement de l’AEM en tant qu’artefacts SDK de Cloud Service et AEM code de projet comme suit :

+ `~/aem-sdk` est un dossier d’organisation contenant les différents outils fournis par AEM as a Cloud Service SDK.
+ `~/aem-sdk/author` contient le service AEM Author
+ `~/aem-sdk/publish` contient le service de publication AEM ;
+ `~/aem-sdk/dispatcher` contient les outils Dispatcher
+ `~/code/<project name>` contient le code source du projet AEM personnalisé.

Notez que `~` est une abréviation du répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%` ;

## Outils de développement pour les projets AEM

Le projet AEM est la base de code personnalisée contenant le code, la configuration et le contenu déployés via Cloud Manager pour AEM en tant que Cloud Service. La structure de base du projet est générée via l’[archétype Maven du projet AEM](https://github.com/adobe/aem-project-archetype).

Cette section du tutoriel explique comment :

+ Installation de la version [!DNL Java]
+ Installer [!DNL Node.js] (et npm)
+ Installation de la version [!DNL Maven]
+ Installation de la version [!DNL Git]

[Configuration des outils de développement pour les projets AEM](./development-tools.md)

## Exécution locale AEM

Le SDK AEM as a Cloud Service fournit une version [!DNL QuickStart Jar] qui exécute une version locale d’AEM. [!DNL QuickStart Jar] peut être utilisé pour exécuter localement le service AEM Author ou le service AEM Publish. Notez que bien que [!DNL QuickStart Jar] fournisse une expérience de développement local, toutes les fonctionnalités disponibles dans AEM en tant que Cloud Service ne sont pas incluses dans [!DNL QuickStart Jar].

Cette section du tutoriel explique comment :

+ Installation de la version [!DNL Java]
+ Téléchargement du SDK AEM
+ Exécutez la commande [!DNL AEM Author Service]
+ Exécutez la commande [!DNL AEM Publish Service]

[Configuration de l’exécution AEM locale](./aem-runtime.md)

## Runtime [!DNL Dispatcher] local

Les outils Dispatcher du SDK d’AEM as a Cloud Service fournissent tout ce qui est nécessaire pour configurer le composant d’exécution local [!DNL Dispatcher]. [!DNL Dispatcher] Les outils sont  [!DNL Docker]basés sur et fournissent des outils de ligne de commande pour transférer les fichiers de serveur  [!DNL Apache HTTP] Web et de  [!DNL Dispatcher] configuration dans des formats compatibles et les déployer pour les  [!DNL Dispatcher] exécuter dans le  [!DNL Docker] conteneur.

Cette section du tutoriel explique comment :

+ Téléchargement du SDK AEM
+ Installer les outils [!DNL Dispatcher]
+ Exécutez le runtime [!DNL Dispatcher] local.

[Configuration de l’exécution Local [!DNL Dispatcher] Runtime](./dispatcher-tools.md)
