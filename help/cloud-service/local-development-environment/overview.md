---
title: Environnement de développement local pour AEM as a Cloud Service
description: Présentation de l’environnement de développement local d’Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 100%

---

# Configurer l’environnement de développement local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Présentation"
>abstract="La configuration de l’environnement de développement local pour AEM as a Cloud Service inclut les outils de développement nécessaires au développement, à la création et à la compilation de projets AEM, ainsi que les délais d’exécution locaux permettant aux développeurs et développeuses de valider rapidement les nouvelles fonctionnalités localement avant de les déployer dans AEM as a Cloud Service via Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=fr" text="Consignes de développement"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=fr" text="Principes de base de développement"

Ce tutoriel vous guide tout au long de la configuration d’un environnement de développement local pour Adobe Experience Manager (AEM) à l’aide du SDK d’AEM as a Cloud Service. Les outils de développement requis pour développer, créer et compiler des projets AEM sont inclus, ainsi que les délais d’exécution locaux permettant aux développeurs et développeuses de valider rapidement les nouvelles fonctionnalités localement avant de les déployer dans AEM as a Cloud Service via Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![Pile technologique de l’environnement de développement local d’AEM as a Cloud Service.](./assets/overview/aem-sdk-technology-stack.png)

L’environnement de développement local pour AEM peut être divisé en trois groupes logiques :

+ Le __projet AEM__ contient le code, la configuration et le contenu personnalisés formant l’application AEM personnalisée.
+ L’__exécution locale d’AEM__ qui exécute une version locale des services AEM Création et Publication localement.
+ L’__exécution locale du Dispatcher__ qui exécute une version locale du serveur web Apache HTTP et du Dispatcher.

Ce tutoriel vous guide tout au long de l’installation et de la configuration des éléments mis en surbrillance dans le diagramme ci-dessus, fournissant ainsi un environnement de développement local stable pour le développement AEM.

## Organisation du système de fichiers

Ce tutoriel a établi l’emplacement des artefacts de SDK d’AEM as a Cloud Service et du code de projet AEM comme suit :

+ `~/aem-sdk` est un dossier d’organisation contenant les différents outils fournis par le SDK d’AEM as a Cloud Service.
+ `~/aem-sdk/author` contient le service AEM Création.
+ `~/aem-sdk/publish` contient le service AEM Publication.
+ `~/aem-sdk/dispatcher` contient les outils du Dispatcher.
+ `~/code/<project name>` contient le code source du projet AEM personnalisé.

`~` est une notation abrégée pour le répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%` ;

## Outils de développement pour les projets AEM

Le projet AEM est la base de code personnalisée contenant le code, la configuration et le contenu déployés via Cloud Manager pour AEM as a Cloud Service. La structure du projet de base est générée via l’[archétype Maven de projet AEM](https://github.com/adobe/aem-project-archetype).

Cette section du tutoriel explique comment :

+ Installer [!DNL Java]
+ Installer [!DNL Node.js] (et npm)
+ Installer [!DNL Maven]
+ Installer [!DNL Git]

[Configurer des outils de développement pour les projets AEM](./development-tools.md)

## Exécution locale d’AEM

Le SDK d’AEM as a Cloud Service fournit un [!DNL QuickStart Jar] qui exécute une version locale d’AEM. Le [!DNL QuickStart Jar] peut être utilisé pour exécuter localement le service AEM Création ou le service AEM Publication. Notez que le [!DNL QuickStart Jar] fournit une expérience de développement local, mais que toutes les fonctionnalités disponibles dans AEM as a Cloud Service ne sont pas incluses dans le [!DNL QuickStart Jar].

Cette section du tutoriel explique comment :

+ Installer [!DNL Java]
+ Télécharger le SDK d’AEM
+ Exécuter le [!DNL AEM Author Service]
+ Exécuter le [!DNL AEM Publish Service]

[Configurer l’exécution locale d’AEM](./aem-runtime.md)

## Exécution locale du [!DNL Dispatcher]

Les outils du Dispatcher du SDK d’AEM as a Cloud Service fournissent tout ce qui est nécessaire à la configuration de l’exécution du [!DNL Dispatcher]. Les outils du [!DNL Dispatcher] sont basés sur [!DNL Docker] et fournissent des outils de ligne de commande pour la transmission du serveur web [!DNL Apache HTTP] et des fichiers de configuration du [!DNL Dispatcher] dans des formats compatibles et leur déploiement dans le [!DNL Dispatcher] en cours d’exécution dans le conteneur [!DNL Docker].

Cette section du tutoriel explique comment :

+ Télécharger le SDK d’AEM
+ Installer les outils du [!DNL Dispatcher]
+ Exécuter localement le [!DNL Dispatcher]

[Configurer l’exécution locale du  [!DNL Dispatcher] ](./dispatcher-tools.md)
