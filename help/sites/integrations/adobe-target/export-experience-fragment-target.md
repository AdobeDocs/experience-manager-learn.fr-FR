---
title: Exporter des fragments d’expérience vers Adobe Target
description: Découvrez comment publier et exporter des fragments d’expérience AEM en tant qu’offres Adobe Target.
feature: Experience Fragments
version: Experience Manager as a Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 100%

---

# Exporter un fragment d’expérience vers Adobe Target {#experience-fragment-target}

Découvrez comment exporter des fragments d’expérience AEM en tant qu’offres Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/328970?quality=12&learn=on&captions=fre_fr)

## Étapes suivantes

+ [Créer une activité Target à l’aide d’offres de fragments d’expérience](./create-target-activity.md)

## Résolution des problèmes

### Échec de l’export de fragments d’expérience vers Target.

#### Erreur

L’export de fragments d’expérience vers Adobe Target sans les autorisations appropriées dans Adobe Admin Console entraîne l’erreur suivante sur le service de création AEM :

![Erreur de l’interface utilisateur de l’API Target.](assets/error-target-offer.png)

... ainsi que les messages de journal suivants dans le journal `aemerror` :

![Erreur de la console de l’API Target.](assets/target-console-error.png)

#### Résolution

1. Connectez-vous à [Admin Console](https://adminconsole.adobe.com/) avec des droits d’administration pour le profil de produit Adobe Target utilisé dans l’intégration AEM.
2. Sélectionnez __Produits > Adobe Target > Profil de produit__.
3. Sous l’onglet __Intégrations__, sélectionnez l’intégration de votre environnement AEM as a Cloud Service (même nom que le projet Adobe Developer).
4. Attribuez le rôle __Éditeur__ ou __Approbateur__.

   ![Erreur de l’API Target.](assets/target-permissions.png).

L’ajout des autorisations appropriées à votre intégration Adobe Target devrait résoudre cette erreur.

## Liens connexes

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
