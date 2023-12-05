---
title: Afficher plusieurs documents PDF
description: Parcourez plusieurs documents PDF dans un formulaire adaptatif.
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 92
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 100%

---

# Afficher plusieurs documents PDF dans un carrousel

Un cas d’utilisation courant consiste à afficher plusieurs documents PDF à l’utilisateur ou l’utilisatrice avant de soumettre le formulaire.

Pour réaliser ce cas d’utilisation, nous avons utilisé l’[API intégré Adobe PDF](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html?lang=fr).

[Vous pouvez voir une démonstration de cet exemple ici.](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

Les étapes suivantes ont été suivies pour effectuer l’intégration :

## Créer un composant personnalisé pour afficher plusieurs documents PDF

Un composant personnalisé (carousel de PDF) a été créé pour parcourir les documents PDF.

## Bibliothèque cliente

Une bibliothèque cliente a été créée pour afficher les PDF à l’aide de l’API d’intégration d’Adobe PDF. Les PDF à afficher sont spécifiés dans les composants de carrousel de PDF.

## Créer un formulaire adaptatif

Créez un formulaire adaptatif basé sur certains onglets (cet exemple comporte 3 onglets).
Ajoutez des composants de formulaire adaptatif dans les deux premiers onglets.
Ajoutez le composant de carrousel de PDF dans le troisième onglet.
Configurez le composant de carrousel de PDF comme illustré dans la copie d’écran ci-dessous.
![pdf-carousel](assets/pdf-carousel-af-component.png)

**Clé de l’API d’intégration de PDF** : il s’agit de la clé que vous pouvez utiliser pour incorporer les PDF. Cette clé ne fonctionne qu’avec localhost. Vous pouvez créer [votre propre clé](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html?lang=fr) et l’associer à un autre domaine.

**Spécifier des documents PDF** : vous pouvez y spécifier les documents PDF que vous souhaitez afficher dans le carrousel.


## Déployer l’exemple sur votre serveur

Pour le tester sur votre serveur local, procédez comme suit :

1. [Importez la bibliothèque cliente](assets/pdf-carousel-client-lib.zip) dans votre instance AEM locale [à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
1. [Importez le composant de carrousel de PDF](assets/pdf-carousel-component.zip) dans votre instance AEM locale [à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
1. [Importez le formulaire adaptatif](assets/adaptive-form-pdf-carousel.zip) dans votre instance AEM locale [à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
1. [Importez les exemples de PDF à afficher](assets/pdf-carousel-sample-documents.zip) dans votre instance AEM locale [à l’aide du lien de chargement du fichier de ressources](http://localhost:4502/assets.html/content/dam).
1. [Prévisualiser le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. Accédez à l’onglet Documents à réviser. Vous devriez voir trois documents PDF dans le composant du carrousel.
