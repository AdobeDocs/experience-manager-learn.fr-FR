---
title: Configurer l’API de communication AEM Forms
description: Configurer des API de communication AEM Forms basées sur OpenAPI pour l’authentification de serveur à serveur
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '245'
ht-degree: 100%

---

# Configurer des API de communication AEM Forms basées sur OpenAPI sur AEM Forms as a Cloud Service

## Prérequis

* Dernière instance d’AEM Forms as a Cloud Service.
* Tous les [profils de produit nécessaires sont ajoutés à l’environnement.](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* Activez l’accès de l’API AEM au profil de produit comme illustré ci-dessous.
  ![product_profile1](assets/product-profiles1.png)
  ![product_profile](assets/product-profiles.png)

## Créer un projet Adobe Developer Console

Connectez-vous à [Adobe Developer Console](https://developer.adobe.com/console/) à l’aide de votre Adobe ID.
Créer un projet en cliquant sur l’icône appropriée.
![new-project](assets/new-project.png)

Attribuez un nom significatif au projet et cliquez sur l’icône Ajouter une API.
![new-project](assets/new-project2.png)

Sélectionnez Experience Cloud.
![new-project3](assets/new-project3.png)
Sélectionnez l’API de communication AEM Forms et cliquez sur Suivant.
![new-project4](assets/new-project4.png)

Vérifiez que vous avez sélectionné l’authentification serveur à serveur, puis cliquez sur Suivant.
![new-project5](assets/new-project5.png)
Sélectionnez les profils et cliquez sur le bouton Enregistrer l’API configurée pour enregistrer vos paramètres.
![new-project6](assets/new-project6.png)
Cliquez sur OAuth serveur à serveur.
![new-project7](assets/new-project7.png)
Copiez l’ID client, le secret client et les portées.
![new-project8](assets/new-project8.png)

## Configurer l’instance AEM pour activer la communication du projet ADC

Si vous disposez déjà d’un projet AEM Forms, [suivez ces instructions](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) pour permettre à l’ID client des informations d’identification OAuth serveur à serveur du projet Adobe Developer Console de communiquer avec l’instance AEM.

Si vous ne disposez pas d’un projet AEM Forms, créez un [projet AEM Forms en suivant cette documentation.](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started), puis activez l’ID client des informations d’identification OAuth serveur à serveur du projet Adobe Developer Console pour qu’il communique avec l’instance AEM [à l’aide de cette documentation](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis).


## Étapes suivantes

[Générer un jeton d’accès](./generate-access-token.md)
