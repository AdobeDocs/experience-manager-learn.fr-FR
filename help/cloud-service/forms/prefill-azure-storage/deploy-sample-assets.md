---
title: Déployer des exemples de ressources
description: Déployez les exemples de ressources sur votre système local compatible avec le cloud.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 44
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 100%

---

# Déployer des exemples de ressources

Pour que ce cas d’utilisation fonctionne sur votre système, déployez les ressources suivantes sur votre système local compatible avec le cloud.

* Assurez-vous d’avoir créé toutes les configurations/tous les comptes requis répertoriés dans le [document d’introduction](./introduction.md).

* [Installer le modèle de formulaire adaptatif personnalisé et le composant de page associé](./assets/azure-portal-template-page-component.zip)

* [Installez le modèle de données de formulaire SendGrid](./assets/send-grid-form-data-model.zip). Vous devrez fournir votre clé d’API et SendGrid vérifié à partir de l’adresse pour que ce modèle de données de formulaire fonctionne. Tester le modèle de données de formulaire dans l’éditeur de modèle de données de formulaire

* [Installation du modèle de données de formulaire pris en charge par Azure](./assets/azure-storage-fdm.zip). Vous devrez fournir vos informations d’identification de compte de stockage Azure pour que le modèle de données de formulaire fonctionne. Testez le modèle de données de formulaire dans l’éditeur de modèle de données de formulaire.

* [Import de l’exemple de formulaire adaptatif](./assets/credit-applications-af.zip)
* [Importer la bibliothèque cliente](./assets/client-lib.zip)
* [Prévisualisation du formulaire](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Saisissez un e-mail valide et cliquez sur le bouton Enregistrer. Les données de formulaire doivent être stockées dans le stockage Azure et un e-mail contenant un lien vers le formulaire enregistré sera envoyé à l’adresse e-mail indiquée.
