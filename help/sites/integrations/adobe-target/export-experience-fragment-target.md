---
title: Exportez des fragments d’expérience vers Adobe Target
description: Découvrez comment publier et exporter AEM fragment d’expérience en tant qu’Offres Adobe Target.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 8%

---


# Exporter le fragment d’expérience vers Adobe Target {#experience-fragment-target}

Découvrez comment exporter AEM fragment d’expérience en tant qu’Offres Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## Étapes suivantes

+ [Création d’une Activité de Cible à l’aide d’Offres de fragments d’expérience](./create-target-activity.md)

## Résolution des incidents

### Echec de l’exportation de fragments d’expérience vers la Cible

#### Erreur

L’exportation du fragment d’expérience vers Adobe Target sans les autorisations appropriées dans Adobe Admin Console entraîne l’erreur suivante sur le service Auteur AEM :

    ![Erreur de l&#39;interface utilisateur de l&#39;API de Cible](assets/error-target-offer.png)

... et les messages de journal suivants dans le journal `aemerror` :

    ![Erreur de la console de l&#39;API de Cible](assets/target-console-error.png)

#### Résolution

1. Connectez-vous à [Admin Console](https://adminconsole.adobe.com/) avec des droits d’administration pour le Profil de produits Adobe Target utilisé mais l’intégration AEM
2. Sélectionnez __Produits > Adobe Target > Profil de produit__
3. Sous l&#39;onglet __Intégrations__, sélectionnez l&#39;intégration de votre AEM en tant qu&#39;environnement Cloud Service (même nom que le projet d&#39;Adobe I/O).
4. Attribuer le rôle __Editor__ ou __approbateur__

   ![Erreur de l&#39;API de cible](assets/target-permissions.png)

Ajouter l’autorisation correcte à votre intégration Adobe Target devrait résoudre cette erreur.

## Liens pris en charge

+ [Débogueur Adobe Experience Cloud - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Débogueur Adobe Experience Cloud - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)