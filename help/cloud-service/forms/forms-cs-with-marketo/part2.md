---
title: Intégration d’AEM Forms Cloud Service et de Marketo (partie 2)
description: Découvrez comment intégrer AEM Forms et Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 13%

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

[Créer un modèle de données de formulaire](./part3.md)