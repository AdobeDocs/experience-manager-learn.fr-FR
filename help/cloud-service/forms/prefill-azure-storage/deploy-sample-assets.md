---
title: Déploiement des exemples de ressources
description: Déployez les exemples de ressources sur votre système cloud local prêt.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# Déploiement des exemples de ressources

Pour que ce cas d’utilisation fonctionne sur votre système, déployez les ressources suivantes sur votre système local prêt pour le cloud.

* Assurez-vous d’avoir créé toutes les configurations/tous les comptes requis répertoriés dans le[document d’introduction](./introduction.md)

* [Installation du modèle de formulaire adaptatif personnalisé et du composant de page associé](./assets/azure-portal-template-page-component.zip)

* [Installation du modèle de données de formulaire SendGrid](./assets/send-grid-form-data-model.zip). Vous devrez fournir votre clé d’API et SendGrid vérifié à partir de l’adresse pour que ce modèle de données de formulaire fonctionne. Tester le modèle de données de formulaire dans l’éditeur de modèle de données de formulaire

* [Installation du modèle de données de formulaire soutenu par Azure](./assets/azure-storage-fdm.zip). Vous devrez fournir vos informations d’identification de compte Azure Storage pour que le modèle de données de formulaire fonctionne. Testez le modèle de données de formulaire dans l’éditeur de modèle de données de formulaire.

* [Importation de l’exemple de formulaire adaptatif](./assets/credit-applications-af.zip)
* [Importation de la bibliothèque cliente](./assets/client-lib.zip)
* [Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Saisissez un e-mail valide et cliquez sur le bouton Enregistrer . Les données de formulaire doivent être stockées dans le stockage Azure et un courrier électronique contenant un lien vers le formulaire enregistré sera envoyé à l’adresse électronique indiquée.
