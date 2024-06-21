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
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Déploiement de votre projet

Avant de commencer à déployer le projet sur votre Cloud Service AEM Forms, il est recommandé de le déployer sur votre instance AEM Forms prête pour le cloud local.

## Synchronisation des modifications avec votre projet AEM

Lancez IntelliJ et accédez au dossier adaptiveForm sous le ``ui.apps`` comme illustré ci-dessous
![intellij](assets/intellij.png)

Clic droit sur ``adaptiveForm`` noeud et sélectionnez Nouveau | Package Veillez à ajouter le nom **address block** au package

Cliquez avec le bouton droit sur le package nouvellement créé. ``addressblock`` et sélectionnez ``repo | Get Command`` comme illustré ci-dessous
![repo-sync](assets/sync-repo.png)

Cela devrait synchroniser le projet avec votre instance AEM Forms locale prête pour le cloud. Vous pouvez vérifier les propriétés du fichier .content.xml .
![after-sync](assets/after-sync.png)

## Déployer le projet sur votre instance locale

Ouvrez une nouvelle fenêtre d’invite de commande, accédez au dossier racine du projet et créez le projet à l’aide de la commande ci-dessous.
![deploy](assets/build-project.png)

Une fois le projet déployé, le composant Adresse peut désormais être utilisé dans un formulaire adaptatif.

## Déploiement du projet dans l’environnement cloud

Si tout s’affiche correctement dans votre environnement de développement local, l’étape suivante consiste à déployer sur la [instance cloud à l’aide de cloud manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



