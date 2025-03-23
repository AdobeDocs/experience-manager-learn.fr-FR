---
title: Création d’un composant d’adresse
description: Création d’un composant principal d’adresse dans AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 100%

---

# Déployer votre projet

Avant de commencer à déployer le projet sur AEM Forms as a Cloud Service, il est recommandé de le déployer sur votre instance AEM Forms locale prête pour le cloud.

## Synchroniser les modifications avec votre projet AEM

Lancez IntelliJ et accédez au dossier adaptiveForm dans le dossier ``ui.apps`` comme illustré ci-dessous.
![intellij](assets/intellij.png)

Cliquez avec le bouton droit sur le nœud ``adaptiveForm`` et sélectionnez Nouveau | Package.
Assurez-vous d’ajouter le nom **addressblock** au package.

Cliquez avec le bouton droit sur le package ``addressblock`` nouvellement créé et sélectionnez ``repo | Get Command`` comme illustré ci-dessous.
![repo-sync](assets/sync-repo.png)

Cela doit synchroniser le projet avec votre instance AEM Forms locale prête pour le cloud. Vous pouvez vérifier et confirmer les propriétés du fichier .content.xml.
![after-sync](assets/after-sync.png)

## Déployer le projet sur votre instance locale

Ouvrez une nouvelle fenêtre d’invite de commandes, accédez au dossier racine du projet et créez le projet à l’aide de la commande ci-dessous.
![deploy](assets/build-project.png)

Une fois le projet déployé, le composant Adresse peut désormais être utilisé dans un formulaire adaptatif.

## Déployer le projet sur l’environnement cloud

Si tout semble correct dans votre environnement de développement local, l’étape suivante consiste à effectuer le déploiement sur [l’instance cloud en utilisant Cloud Manager.](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
