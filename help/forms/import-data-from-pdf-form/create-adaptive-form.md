---
title: Créer un schéma
description: Créer un schéma basé sur les données à importer dans le formulaire adaptatif
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 20%

---

# Présentation

La première étape consiste à créer un schéma basé sur les données qui seront utilisées pour remplir le formulaire adaptatif.

## XFA est basé sur un schéma

Utilisation du schéma pour créer votre formulaire adaptatif

## XFA ne repose pas sur un schéma

* Ouvrez le fichier XDP dans AEM Forms Designer.
* Cliquez sur Fichier | Propriétés du formulaire | Aperçu..
* Cliquez sur Générer les données d’aperçu.
* Cliquez sur Générer.
* Indiquez un nom de fichier significatif, tel que `form-data.xml`

Vous pouvez utiliser n’importe quel outil en ligne gratuit pour [générer le XSD](https://www.freeformatter.com/xsd-generator.html) à partir des données xml générées à l’étape précédente.

Créez un formulaire adaptatif basé sur le schéma de l’étape précédente.

>[!NOTE]
>Il est toujours recommandé d’examiner les données générées lors de l’envoi du formulaire adaptatif. Vous aurez ainsi une bonne idée du format XML des données qui doivent être fusionnées avec le formulaire adaptatif.

Données envoyées depuis un formulaire adaptatif
![submit-data](./assets/af-submitted-data.png)

Données exportées depuis le PDF
![excluded-data](./assets/exported-data.png)

A partir des données exportées, vous devez extraire la variable **_topmostSubform_** noeud avec les espaces de noms appropriés conservés pour fusionner les données avec le formulaire adaptatif.

## Étapes suivantes

[Créer un service OSGi](./create-osgi-service.md)





