---
title: AEM Forms avec Marketo (Partie 2)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 137
source-git-commit: 8bde459ae9a6e261cfc3aff308babe9de6e56059
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 15%

---

# Créer une source de données

Les API REST Marketo sont authentifiées avec OAuth 2.0 à 2 jambes. Nous pouvons facilement créer une source de données à l’aide du fichier swagger téléchargé à l’étape précédente.

## Création d’un conteneur de configuration

* Connectez-vous à AEM.
* Cliquez sur le menu Outils, puis sur **Explorateur de configurations** comme illustré ci-dessous.

* ![menu des outils](assets/datasource3.png)

* Cliquez sur **Créer** et indiquez un nom significatif comme illustré ci-dessous. Veillez à sélectionner l’option Configurations du cloud comme illustré ci-dessous.

* ![conteneur de configuration](assets/datasource4.png)

## Création de services cloud

* Accédez au menu Outils , puis cliquez sur Services cloud -> Sources de données

* ![cloud-services](assets/datasource5.png)

* Sélectionnez le conteneur de configuration créé à l’étape précédente et cliquez sur **Créer** pour créer une source de données. Attribuez un nom significatif, sélectionnez le service RESTful dans la liste déroulante Type de service et cliquez sur **Suivant**
* ![new-data-source](assets/datasource6.png)

* Téléchargez le fichier swagger et spécifiez le type d’octroi, l’ID client, le secret client et l’URL du jeton d’accès spécifiques à votre instance Marketo, comme illustré dans la capture d’écran ci-dessous.

* Testez la connexion et, si la connexion est établie, veillez à cliquer sur le bouton bleu **Créer** pour terminer le processus de création de la source de données.

* ![data-source-config](assets/datasource1.png)


## Étapes suivantes

[Créer une source de données basée sur le service RESTful](./part3.md)
