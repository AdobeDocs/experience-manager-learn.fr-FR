---
title: Exporter des fragments d’expérience vers Adobe Target
description: Découvrez comment publier et exporter des fragments d’expérience AEM en tant qu’offres Adobe Target.
feature: Experience Fragments
version: Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '180'
ht-degree: 100%

---

# Exporter un fragment d’expérience vers Adobe Target {#experience-fragment-target}

Découvrez comment exporter des fragments d’expérience AEM en tant qu’offres Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

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
