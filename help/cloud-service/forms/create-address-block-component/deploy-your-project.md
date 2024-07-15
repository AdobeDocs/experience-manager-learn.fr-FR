---
title: Création du composant Adresse
description: Création d’un composant principal d’adresse dans AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: a12b1778413079646814cb25567abfc26a429340
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Déploiement de votre projet

Avant de commencer à déployer le projet sur votre Cloud Service AEM Forms, il est recommandé de le déployer sur votre instance AEM Forms prête pour le cloud local.

## Synchronisation des modifications avec votre projet AEM

Lancez IntelliJ et accédez au dossier adaptiveForm sous le dossier ``ui.apps`` comme illustré ci-dessous.
![intellij](assets/intellij.png)

Cliquez avec le bouton droit sur le noeud ``adaptiveForm`` et sélectionnez Nouveau | Package
Veillez à ajouter le nom **address block** au module

Cliquez avec le bouton droit sur le package nouvellement créé ``addressblock`` et sélectionnez ``repo | Get Command`` comme illustré ci-dessous.
![repo-sync](assets/sync-repo.png)

Cela devrait synchroniser le projet avec votre instance AEM Forms locale prête pour le cloud. Vous pouvez vérifier les propriétés du fichier .content.xml .
![after-sync](assets/after-sync.png)

## Déployer le projet sur votre instance locale

Ouvrez une nouvelle fenêtre d’invite de commande, accédez au dossier racine du projet et créez le projet à l’aide de la commande ci-dessous.
![deploy](assets/build-project.png)

Une fois le projet déployé, la variable
Le composant Adresse peut désormais être utilisé dans un formulaire adaptatif

## Déploiement du projet dans l’environnement cloud

Si tout semble correct dans votre environnement de développement local, l’étape suivante consiste à déployer sur l’instance [cloud en utilisant le gestionnaire de cloud.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
