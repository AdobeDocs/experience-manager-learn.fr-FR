---
title: Créer un schéma
description: Créez un schéma basé sur les données à importer dans le formulaire adaptatif.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 53
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 97%

---

# Présentation

La première étape consiste à créer un schéma basé sur les données qui seront utilisées pour remplir le formulaire adaptatif.

## XFA repose sur un schéma

Utilisez le schéma pour créer votre formulaire adaptatif.

## XFA ne repose pas sur un schéma

* Ouvrez le fichier XDP dans AEM Forms Designer.
* Cliquez sur Fichier | Propriétés du formulaire | Aperçu.
* Cliquez sur Générer les données d’aperçu.
* Cliquez sur Générer.
* Fournissez un nom de fichier pertinent, tel que `form-data.xml`.

Vous pouvez utiliser n’importe quel outil en ligne gratuit pour [générer le XSD](https://www.freeformatter.com/xsd-generator.html) à partir des données xml générées à l’étape précédente.

Créez un formulaire adaptatif basé sur le schéma de l’étape précédente.

>[!NOTE]
>Il est toujours recommandé d’examiner les données générées lors de l’envoi du formulaire adaptatif. Vous aurez ainsi une bonne idée du format XML des données qui doivent être fusionnées au formulaire adaptatif.

Données envoyées d’un formulaire adaptatif
![submit-data](./assets/af-submitted-data.png)

Données exportées du PDF
![excluded-data](./assets/exported-data.png)

À partir des données exportées, vous devez extraire le nœud **_topmostSubform_** avec les espaces de noms appropriés conservés pour fusionner les données avec le formulaire adaptatif.

## Étapes suivantes

[Créer un service OSGi](./create-osgi-service.md)
