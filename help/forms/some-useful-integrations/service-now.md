---
title: Intégration avec [!DNL ServiceNow]
description: Créez et affichez tous les incidents à l’aide du modèle de données de formulaire.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 5%

---

# Intégration d’AEM Forms à [!DNL ServiceNow]

Créer et afficher un incident dans [!DNL ServiceNow] utilisation du modèle de données de formulaire dans AEM Forms.

## Conditions préalables

* [!DNL ServiceNow] compte .
* Familiarisez-vous avec [création de sources de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarisez-vous avec [Modèle de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr)

## Exemples de ressources

Les exemples de ressources fournis avec cet article sont les suivants :

* Configuration du service cloud
* Fichiers Swagger pour créer un incident et récupérer tous les incidents
* Modèle de données de formulaire basé sur les fichiers swagger
* Formulaire adaptatif à créer et à répertorier [!DNL ServiceNow] incidents

## Déployer les ressources sur votre serveur

* Téléchargez la [exemple de ressources](assets/service-now.zip)
* Importez les ressources dans AEM à l’aide de [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* Le fichier swagger utilisé pour cette intégration se trouve sous la propriété ```/conf/9957/settings/cloudconfigs/fdm``` dossier dans le référentiel crx
* Modifiez la variable [Configuration du service cloud CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)pour correspondre à votre instance ServiceNow.
* Modifiez la variable [Configuration du service cloud GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) pour correspondre à votre instance ServiceNow. Vous devez modifier l’hôte, le nom d’utilisateur et le mot de passe pour qu’ils correspondent aux informations d’identification de votre instance ServiceNow.

## Accès aux informations d’identification de l’instance ServiceNow

* Clic sur votre profil utilisateur
   ![clic sur profil utilisateur](assets/snow-1.png)

* Cliquez sur Gérer le mot de passe de l’instance.
* Les détails de l’instance sont présentés ci-dessous.
   ![détails de l’instance](assets/snow-3.png)

## Test de l’intégration

* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Saisissez certaines valeurs dans le champ de description et de commentaires, puis cliquez sur le bouton Créer un incident .
* L’identifiant d’incident du nouvel incident doit être renseigné dans le champ de texte et le tableau ci-dessous doit répertorier tous les incidents.
