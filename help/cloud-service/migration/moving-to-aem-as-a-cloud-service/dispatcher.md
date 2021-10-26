---
title: Configuration de Dispatcher lors du passage à AEM as a Cloud Service
description: Découvrez les modifications notables apportées à AEM Dispatcher pour AEM as a Cloud Service, l’outil de conversion de Dispatcher et comment utiliser le SDK des outils de Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 53a314c5cd9eaad5a26a0992c750c159f8e3697f
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 2%

---

# Dispatcher

Découvrez AEM Dispatcher pour AEM as a Cloud Service, en vous concentrant sur les modifications notables apportées par Dispatcher pour la version 6, l’outil de conversion de Dispatcher et sur l’utilisation du SDK des outils Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Convertisseur du Dispatcher

![Convertisseur du Dispatcher](./assets/dispatcher-converter-diagram.png)

Dans le cadre de la refactorisation de votre base de code, utilisez le [Convertisseur du Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) pour refactoriser les configurations Dispatcher On-Premise ou Adobe Managed Services existantes afin d’AEM la configuration Dispatcher compatible as a Cloud Service.

### Principales activités

* Utilisez la variable [Outil convertisseur du Dispatcher Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) pour migrer une configuration Dispatcher existante.
* Référencez le module de Dispatcher à partir de [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) en tant que bonne pratique.
* [Configuration des outils Dispatcher locaux](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) pour valider le dispatcher, avant de le tester dans un environnement de Cloud Service.


