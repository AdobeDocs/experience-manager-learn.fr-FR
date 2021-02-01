---
title: Acroformes avec AEM Forms
description: Didacticiel qui décrit comment créer un formulaire adaptatif à l’aide d’Acrobat et fusionner les données pour obtenir un PDF. Le PDF contenant les données fusionnées peut alors être envoyé pour signature à l’aide d’Adobe Sign.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 4%

---


# Création de Forms adaptative à partir d’Acrobat

Les organisations ont une grande variété de formes. Certains de ces formulaires sont créés dans Microsoft Word et convertis en PDF. Par défaut, ces formulaires ne peuvent pas être remplis à l’aide de Adobe Reader ou Acrobat. Pour que ces formulaires puissent être remplis à l’aide de l’Acrobat ou du Reader, nous devons les convertir en Acroform. Les Acroforms sont des formulaires créés à l’aide d’Acrobat. Cet article décrit comment créer un formulaire adaptatif à partir d’Acrobat et fusionner les données dans Acrobat pour obtenir le PDF. Le PDF contenant les données fusionnées peut également être envoyé pour signature à l’aide d’Adobe Sign.

>[!NOTE]
>
>Si vous utilisez AEM Forms 6.5, utilisez la fonctionnalité Automated forms conversion.

## Conditions préalables

* AEM Forms 6.3 ou 6.4 installé et configuré
* Accès à Adobe Acrobat
* Connaissance de AEM/AEM Forms.

### Les éléments suivants sont nécessaires pour que cette fonctionnalité fonctionne sur votre système.

* Téléchargez et déployez les lots à l’aide de la [console Web Felix](http://localhost:4502/system/console/bundles).
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [Téléchargez et importez ce package dans AEM](assets/acro-form-aem-form.zip). Ce module contient l’exemple de flux de travail et de page html pour créer un schéma XSD à partir d’acroform.
* Ouvrez [configMgr](http://localhost:4502/system/console/configMgr)
   * Recherchez &quot;Service Apache Sling User Mapper Service&quot; et cliquez sur pour ouvrir les propriétés.
   * Cliquez sur l&#39;icône `+` (plus) pour ajouter le mappage de services suivant.
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * Cliquez sur &quot;Enregistrer&quot;.
