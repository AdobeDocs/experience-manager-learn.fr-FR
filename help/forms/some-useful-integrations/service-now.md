---
title: Intégrer à  [!DNL ServiceNow]
description: Créez et affichez tous les incidents à l’aide du modèle de données de formulaire.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 100%

---

# Intégrer AEM Forms à [!DNL ServiceNow]

Créez et affichez un incident dans [!DNL ServiceNow] à l’aide du modèle de données de formulaire dans AEM Forms.

## Conditions préalables

* Compte [!DNL ServiceNow].
* Se familiariser avec la [création de sources de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=fr)
* Se familiariser avec le [modèle de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr)

## Exemples de ressources

Les exemples de ressources fournis avec cet article sont les suivants :

* Configuration du service cloud.
* Fichiers Swagger pour créer un incident et récupérer tous les incidents.
* Modèle de données de formulaire basé sur les fichiers Swagger.
* Formulaire adaptatif pour créer et répertorier des incidents [!DNL ServiceNow].

## Déployer les ressources sur votre serveur

* Téléchargez les [exemple de ressources](assets/service-now.zip).
* Importez les ressources dans AEM à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Le fichier Swagger utilisé pour cette intégration se trouve dans le dossier ```/conf/9957/settings/cloudconfigs/fdm``` dans le référentiel crx.
* Modifiez la [configuration du service cloud de CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)pour qu’elle corresponde à votre instance ServiceNow.
* Modifiez la [configuration du service cloud de GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) pour qu’elle corresponde à votre instance ServiceNow. Vous devez modifier l’hôte, le nom d’utilisateur ou d’utilisatrice et le mot de passe pour qu’ils correspondent aux informations d’identification de votre instance ServiceNow.

## Accéder aux informations d’identification de l’instance ServiceNow

* Cliquez sur votre profil utilisateur ou utilisatrice.
  ![Clic sur le profil utilisateur ou utilisatrice.](assets/snow-1.png)

* Cliquez sur Gérer le mot de passe de l’instance.
* Les détails de l’instance sont présentés ci-dessous.
  ![Détails de l’instance.](assets/snow-3.png)

## Test de l’intégration

* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Saisissez des valeurs dans le champ de description et de commentaires, puis cliquez sur le bouton Créer un incident.
* L’ID du nouvel incident doit être renseigné dans le champ de texte et le tableau ci-dessous doit répertorier tous les incidents.
