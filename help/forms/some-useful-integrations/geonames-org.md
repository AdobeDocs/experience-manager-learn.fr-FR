---
title: Listes déroulantes en cascade
description: Renseignez les listes déroulantes en fonction d’une sélection préalable de liste déroulante.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 100%

---

# Listes déroulantes en cascade

Une liste déroulante en cascade est une série de contrôles DropDownList dépendants dans laquelle un contrôle DropDownList dépend des contrôles DropList parents ou précédents. Les éléments du contrôle DropDownList sont renseignés en fonction d’un élément sélectionné par l’utilisateur ou l’utilisatrice à partir d’un autre contrôle DropDownList.

## Démonstration du cas d’utilisation

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

Dans ce tutoriel, j’ai utilisé l’[API REST Geonames](http://api.geonames.org/) pour démontrer cette fonctionnalité.
Un certain nombre d’organisations proposent ce type de service. Tant que vous disposez d’API REST bien documentées, vous pouvez facilement intégrer AEM Forms à l’aide de la fonctionnalité d’intégration des données.

Les étapes suivantes servent à mettre en œuvre des listes déroulantes en cascade dans AEM Forms.

## Créer un compte de développement

Créez un compte de développement avec [Geonames](https://www.geonames.org/login). Notez le nom d’utilisateur ou d’utilisatrice. Ce nom d’utilisateur ou d’utilisatrice est nécessaire pour appeler les API REST de geonames.org.

## Créer un fichier Swagger/OpenAPI

La spécification OpenAPI (anciennement spécification Swagger) est un format de description d’API pour les API REST. Un fichier OpenAPI vous permet de décrire l’ensemble de votre API, notamment :

* les points d’entrée (/users) et les opérations disponibles sur chaque point d’entrée (GET/users, POST /users) ;
* les paramètres d’opération d’entrée et de sortie pour chaque opération.
Méthodes d’authentification
* Coordonnées, licences, conditions d’utilisation et autres informations.
* Les spécifications d’API peuvent être écrites en YAML ou JSON. Ce format est facile à apprendre et à lire pour les humains comme pour les machines.

Pour créer votre premier fichier Swagger/OpenAPI, suivez la procédure décrite dans la [documentation OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/).

>[!NOTE]
> AEM Forms prend en charge la spécification OpenAPI version 2.0 (FKA Swagger).

Utilisez l’[éditeur de Swagger](https://editor.swagger.io/) pour créer votre fichier Swagger afin de décrire les opérations qui récupèrent tous les pays et éléments enfants du pays ou de l’État. Le fichier Swagger peut être créé au format JSON ou YAML.

## Créer des sources de données

Pour intégrer AEM/AEM Forms à des applications tierces, nous devons [créer une source de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=fr) dans la configuration des services cloud. Veuillez utiliser les [fichiers Swagger](assets/geonames-swagger-files.zip) pour créer vos sources de données.
Vous devez créer 2 sources de données (l’une pour récupérer tous les pays et l’autre pour obtenir des éléments enfants).


## Créer un modèle de données de formulaire

L’intégration de données d’AEM Forms fournit une interface utilisateur intuitive permettant de créer et d’utiliser des [modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr). Basez le modèle de données de formulaire sur les sources de données créées à l’étape précédente. Modèle de données de formulaire avec 2 sources de données

![Modèle de données de formulaire.](assets/geonames-fdm.png)


## Créer un formulaire adaptatif

Intégrez les appels GET du modèle de données de formulaire à votre formulaire adaptatif pour renseigner les listes déroulantes.
Créez un formulaire adaptatif avec 2 listes déroulantes. Une liste pour répertorier les pays, et l’autre pour répertorier les États/provinces en fonction du pays sélectionné.

### Renseigner la liste déroulante Pays

La liste des pays est renseignée lors de la première initialisation du formulaire. La capture d’écran suivante montre l’éditeur de règles configuré pour renseigner les options de la liste déroulante des pays. Pour que cela fonctionne, vous devrez fournir votre nom d’utilisateur ou d’utilisatrice avec le compte Geonames.
![get-countries](assets/get-countries-rule-editor.png)

#### Renseigner la liste déroulante État/Province

La liste déroulante État/Province doit être renseignée en fonction du pays sélectionné. La capture d’écran suivante présente la configuration de l’éditeur de règles.
![state-province-options](assets/state-province-options.png)

### Exercice

Ajoutez 2 listes déroulantes appelées Villes et Départements dans le formulaire pour répertorier les villes et départements en fonction du pays et de l’État ou de la province sélectionnés.
![Exercice.](assets/cascading-drop-down-exercise.png)


### Exemples de ressources

Vous pouvez télécharger les ressources suivantes pour commencer à créer une liste déroulante en cascade.
Les fichiers Swagger complets sont disponibles [ici](assets/geonames-swagger-files.zip).
Les fichiers Swagger décrivent l’API REST suivante.
* [Obtenir tous les pays](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Obtenir les enfants de l’objet Geoname](http://api.geonames.org/children?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

Le [modèle de données de formulaire complété peut être téléchargé ici](assets/geonames-api-form-data-model.zip).
