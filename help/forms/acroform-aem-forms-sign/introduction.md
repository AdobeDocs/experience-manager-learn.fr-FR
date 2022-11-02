---
title: Acroforms avec AEM Forms
description: Tutoriel qui décrit la création d’un formulaire adaptatif à l’aide d’Acrobat et la fusion des données pour obtenir un PDF. Le PDF contenant les données fusionnées peut ensuite être envoyé pour signature à l’aide d’Acrobat Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 4%

---


# Création de Forms adaptatif à partir d’Acrobat

Les organisations ont une grande variété de formes. Certains de ces formulaires sont créés dans Microsoft Word et convertis en PDF. Par défaut, ces formulaires ne peuvent pas être remplis à l’aide d’Adobe Reader ou d’Acrobat. Pour que ces formulaires puissent être remplis à l’aide d’Acrobat ou de Reader, nous devons les convertir en Acrobat. Les Acroforms sont des formulaires créés à l’aide d’Acrobat. Cet article décrit la création d’un formulaire adaptatif à partir d’Acrobat et la fusion des données dans Acrobat pour obtenir le PDF. Le PDF avec les données fusionnées peut également être envoyé pour signature à l’aide d’Acrobat Sign.

>[!NOTE]
>
>Si vous utilisez AEM Forms 6.5, utilisez la fonctionnalité d’Automated forms conversion.

## Prérequis

* AEM Forms 6.3 ou 6.4 installé et configuré
* Accès à Adobe Acrobat
* Familiarité avec AEM/AEM Forms.

### Les éléments suivants sont nécessaires pour que cette fonctionnalité fonctionne sur votre système :

* Téléchargez et déployez les lots à l’aide du [Console web Felix](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Téléchargez et importez ce package dans AEM](assets/acro-form-aem-form.zip). Ce module contient l’exemple de workflow et de page HTML pour créer un fichier XSD à partir d’acroform.
* Ouvrez le [configMgr](http://localhost:4502/system/console/configMgr)
   * Recherchez &quot;Service de mappage des utilisateurs du service Apache Sling&quot; et cliquez pour ouvrir les propriétés.
   * Cliquez sur le bouton `+` (plus) pour ajouter le mappage de service suivant
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Cliquez sur &quot;Enregistrer&quot;.
