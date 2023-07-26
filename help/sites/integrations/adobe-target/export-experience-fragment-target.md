---
title: Exporter des fragments d’expérience vers Adobe Target
description: Découvrez comment publier et exporter AEM fragment d’expérience en tant qu’offres Adobe Target.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 6%

---

# Exportation du fragment d’expérience vers Adobe Target {#experience-fragment-target}

Découvrez comment exporter AEM fragment d’expérience en tant qu’offres Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Étapes suivantes

+ [Création d’une activité Target à l’aide d’offres de fragments d’expérience](./create-target-activity.md)

## Résolution des problèmes

### Échec de l’exportation des fragments d’expérience vers Target

#### Erreur

L’exportation de fragments d’expérience vers Adobe Target sans les autorisations appropriées dans Adobe Admin Console entraîne l’erreur suivante sur le service AEM Author :

    ![Erreur de l’interface utilisateur de l’API Target](assets/error-target-offer.png)

... et les messages de journal suivants dans la variable `aemerror` log :

    ![Erreur de la console API de Target](assets/target-console-error.png)

#### Résolution

1. Connectez-vous à [Admin Console](https://adminconsole.adobe.com/) avec des droits d’administration pour le profil de produit Adobe Target utilisé, mais l’intégration AEM
2. Sélectionner __Produits > Adobe Target > Profil de produit__
3. Sous __Intégrations__ , sélectionnez l’intégration de votre environnement as a Cloud Service AEM (même nom que le projet Adobe I/O).
4. Attribuer __Éditeur__ ou __Approbateur__ rôle

   ![Erreur de l’API Target](assets/target-permissions.png)

L’ajout de l’autorisation correcte à votre intégration Adobe Target devrait résoudre cette erreur.

## Liens pris en charge

+ [Débogueur Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Débogueur Adobe Experience Cloud - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
