---
title: AcroForms avec AEM Forms
description: Tutoriel décrivant la création d’un formulaire adaptatif à l’aide d’AcroForm, ainsi que la fusion des données pour obtenir un PDF. Le PDF contenant les données fusionnées peut ensuite être envoyé pour signature à l’aide d’Acrobat Sign.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 52
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 100%

---


# Créer des formulaires adaptatifs à partir d’AcroForms

Les organisations utilisent un grand nombre de formulaires. Certains sont créés dans Microsoft Word et convertis en PDF. Par défaut, ces formulaires ne peuvent pas être remplis à l’aide d’Adobe Reader ou d’Acrobat. Pour ce faire, ils doivent d’abord être convertis en AcroForms. Les AcroForms sont des formulaires créés à l’aide d’Acrobat. Cet article décrit la création d’un formulaire adaptatif à partir d’AcroForm et la fusion des données dans AcroForm pour obtenir le PDF. Le PDF contenant les données fusionnées peut également être envoyé pour signature à l’aide d’Acrobat Sign.

>[!NOTE]
>
>Si vous utilisez AEM Forms 6.5, aidez-vous de la fonction de conversion automatisée de formulaires.

## Conditions préalables

* AEM Forms 6.3 ou 6.4 installé et configuré
* Accès à Adobe Acrobat
* Bonnes connaissances d’AEM/AEM Forms.

### Les éléments suivants sont nécessaires pour que cette fonctionnalité soit activée sur votre système :

* Téléchargez et déployez les lots à l’aide de la [Console web Felix](http://localhost:4502/system/console/bundles).
* [DocumentServicesBundle.](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar).
* [Téléchargez et importez ce package dans AEM](assets/acro-form-aem-form.zip). Ce package contient l’exemple de workflow et de page HTML permettant de créer un fichier XSD à partir d’AcroForm.
* Ouvrez [configMgr](http://localhost:4502/system/console/configMgr).
   * Recherchez « service de mappage utilisateur de service Apache Sling » et cliquez pour ouvrir les propriétés.
   * Cliquez sur l’icône `+` (plus) pour ajouter le mappage de service suivant.
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Cliquez sur Enregistrer.
