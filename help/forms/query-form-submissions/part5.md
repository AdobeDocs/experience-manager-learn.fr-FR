---
title: Déployer l’exemple sur le serveur local
description: Tutoriel en plusieurs parties détaillant les étapes nécessaires pour interroger les envois de formulaire stockés dans Azure Portal
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 100%

---

# Déployer l’exemple sur le serveur local

Pour que ce cas d’utilisation fonctionne sur votre serveur local, suivez les étapes ci-dessous. Il est supposé que votre instance d’AEM s’exécute sur localhost, port 4502.

* [Installez le package](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) à l’aide du gestionnaire de modules.

* Fournissez les informations d’identification du portail Azure à l’aide de configMgr OSGi.
  ![azure-portal](assets/azure-portal-config.png)
Assurez-vous que l’URI de stockage se termine par une barre oblique et que le jeton SAS commence par un ?.
* Accédez à [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo).

* Modifiez les paramètres d’authentification des trois sources de données suivantes pour qu’elles correspondent à votre environnement.
  ![data-sources](assets/fdm-data-sources.png)

* Prévisualiser et soumettre le [formulaire ContactUs](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Interroger l’envoi de votre formulaire](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
