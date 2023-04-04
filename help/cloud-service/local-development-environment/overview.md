---
title: Environnement de développement local pour AEM as a Cloud Service
description: Présentation de l’environnement de développement local d’Adobe Experience Manager (AEM).
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 6%

---

# Configuration de l’environnement de développement local {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="Présentation"
>abstract="La configuration de l’environnement de développement local pour AEM as a Cloud Service inclut les outils de développement nécessaires au développement, à la création et à la compilation de AEM projets, ainsi que les délais d’exécution locaux permettant aux développeurs de valider rapidement les nouvelles fonctionnalités localement avant de les déployer vers l’as a Cloud Service via Adobe Cloud Manager."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=fr" text="Conseils de développement"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=fr" text="Bases du développement"

Ce tutoriel décrit la configuration d’un environnement de développement local pour Adobe Experience Manager (AEM) à l’aide du SDK as a Cloud Service AEM. Les outils de développement requis pour développer, créer et compiler des projets AEM, ainsi que les heures d’exécution locales, permettent aux développeurs de valider rapidement les nouvelles fonctionnalités localement avant de les déployer dans AEM as a Cloud Service via Adobe Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM Pile technologique as a Cloud Service de l&#39;environnement de développement local](./assets/overview/aem-sdk-technology-stack.png)

L’environnement de développement local pour AEM peut être divisé en trois groupes logiques :

+ Le __AEM projet__ contient le code, la configuration et le contenu personnalisés de l’application d’AEM personnalisée.
+ Le __Exécution locale AEM__ qui exécute localement une version locale des services AEM Author et Publish.
+ Le __Exécution locale du Dispatcher__ qui exécute une version locale du serveur web Apache HTTP et de Dispatcher.

Ce tutoriel explique comment installer et configurer les éléments mis en surbrillance dans le diagramme ci-dessus, fournissant ainsi un environnement de développement local stable pour le développement AEM.

## Organisation du système de fichiers

Ce tutoriel a établi l’emplacement des artefacts de SDK as a Cloud Service AEM et du code de projet AEM comme suit :

+ `~/aem-sdk` est un dossier d’organisation contenant les différents outils fournis par le SDK as a Cloud Service AEM
+ `~/aem-sdk/author` contient le service AEM Author
+ `~/aem-sdk/publish` contient le service de publication AEM ;
+ `~/aem-sdk/dispatcher` contient les outils Dispatcher
+ `~/code/<project name>` contient le code source du projet AEM personnalisé.

Notez que `~` est abrégé pour le répertoire de l’utilisateur. Sous Windows, il s’agit de l’équivalent de `%HOMEPATH%`;

## Outils de développement pour les projets AEM

Le projet AEM est la base de code personnalisée contenant le code, la configuration et le contenu déployés via Cloud Manager pour AEM as a Cloud Service. La structure de base du projet est générée via le [Archétype Maven de projet AEM](https://github.com/adobe/aem-project-archetype).

Cette section du tutoriel explique comment :

+ Installation de la version [!DNL Java]
+ Installer [!DNL Node.js] (et npm)
+ Installation de la version [!DNL Maven]
+ Installation de la version [!DNL Git]

[Configuration des outils de développement pour les projets AEM](./development-tools.md)

## Exécution locale AEM

Le SDK as a Cloud Service AEM fournit une [!DNL QuickStart Jar] qui exécute une version locale d’AEM. Le [!DNL QuickStart Jar] peut être utilisé pour exécuter localement le service d’auteur AEM ou le service de publication AEM. Notez que lorsque la variable [!DNL QuickStart Jar] fournit une expérience de développement locale, mais toutes les fonctionnalités disponibles dans AEM as a Cloud Service ne sont pas incluses dans la variable [!DNL QuickStart Jar].

Cette section du tutoriel explique comment :

+ Installation de la version [!DNL Java]
+ Téléchargement du SDK AEM
+ Exécutez la variable [!DNL AEM Author Service]
+ Exécutez la variable [!DNL AEM Publish Service]

[Configuration de l’exécution AEM locale](./aem-runtime.md)

## Local [!DNL Dispatcher] Exécution

AEM outils Dispatcher du SDK as a Cloud Service fournit tout ce qui est nécessaire pour configurer le paramètre local [!DNL Dispatcher] runtime. [!DNL Dispatcher] Les outils sont [!DNL Docker]Basé sur et fournit des outils de ligne de commande pour la transmission [!DNL Apache HTTP] Serveur web et [!DNL Dispatcher] fichiers de configuration dans des formats compatibles et les déployer sur [!DNL Dispatcher] en cours d’exécution dans la variable [!DNL Docker] conteneur.

Cette section du tutoriel explique comment :

+ Téléchargement du SDK AEM
+ Installer [!DNL Dispatcher] Outils
+ Exécutez le fichier local [!DNL Dispatcher] runtime

[Configuration du paramètre local [!DNL Dispatcher] Exécution](./dispatcher-tools.md)
