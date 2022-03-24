---
title: Intégration avec Service Now
description: Créez et affichez tous les incidents à l’aide du modèle de données de formulaire.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 3%

---

# Intégration d’AEM Forms avec le service

Créez et affichez un incident dans ServiceNow à l’aide du modèle de données de formulaire dans AEM Forms.

## Prérequis

* Compte ServiceNow .
* Familiarisez-vous avec [création de sources de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarisez-vous avec [Modèle de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Exemples de ressources

Les exemples de ressources fournis avec cet article sont les suivants :
* Configuration du service cloud
* Fichiers Swagger pour créer un incident et récupérer tous les incidents
* Modèle de données de formulaire basé sur les fichiers swagger
* Formulaire adaptatif pour créer et répertorier les incidents actuels du service

## Déployer les ressources sur votre serveur

* Téléchargez la [exemple de ressources](assets/service-now.zip)
* Importez les ressources dans AEM à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Modifiez la variable [Configuration du service cloud CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)pour correspondre à votre instance ServiceNow.
* Modifiez la variable [Configuration du service cloud GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) pour correspondre à votre instance ServiceNow


## Test de l’intégration

* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Saisissez certaines valeurs dans le champ de description et de commentaires, puis cliquez sur le bouton Créer un incident .
* L’identifiant d’incident du nouvel incident doit être renseigné dans le champ de texte et le tableau ci-dessous doit répertorier tous les incidents.

