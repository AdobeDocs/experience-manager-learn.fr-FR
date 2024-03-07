---
title: Déployer l’exemple sur votre serveur local
description: Tutoriel en plusieurs parties pour vous guider à travers les étapes impliquées dans l’interrogation des envois de formulaire stockés dans Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Déployer l’exemple sur votre serveur local

Pour que ce cas d’utilisation fonctionne sur votre serveur local, suivez les étapes ci-dessous. Votre instance d’AEM s’exécute sur localhost, port 4502.

* [Installation du package](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) à l’aide de package manager.

* Fournissez les informations d’identification du portail Azure à l’aide de la configurationMgr OSGi.
  ![azure-portal](assets/azure-portal-config.png)
Assurez-vous que l’URI de stockage se termine par une barre oblique et que le jeton SAS commence par un ?
* Accédez à [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Modifiez les paramètres d’authentification des trois sources de données suivantes pour qu’elles correspondent à votre environnement.
  ![data-sources](assets/fdm-data-sources.png)

* Aperçu et envoi [Formulaire de contact](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Interrogation de votre envoi de formulaire](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

