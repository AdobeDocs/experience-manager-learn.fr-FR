---
title: Acroforms avec AEM Forms
seo-title: Fusion de données de formulaire adaptatif avec Acrobat
description: Partie 2 de l’intégration d’Acrobat à AEM Forms. Créez un schéma à partir d’un Acroform.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 2%

---


# Création d’un schéma à partir de l’acroformulaire

L’étape suivante consiste à créer un schéma à partir de l’Acrobat créé à l’étape précédente. Un exemple d’application est fourni pour créer le schéma dans le cadre de ce tutoriel. Pour créer le schéma, suivez les instructions suivantes :

1. Connectez-vous à [CRXDE Lite](http://localhost:4502/crx/de)
2. Ouvrez le fichier `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Remplacez `saveLocation` par le dossier approprié de votre disque dur. Assurez-vous que le dossier dans lequel vous enregistrez est déjà créé.
4. Pointez votre navigateur sur la page [Créer XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) hébergée sur AEM.
5. Faites glisser et déposez l’Acroform.
6. Vérifiez le dossier spécifié à l’étape 3. Le fichier de schéma est enregistré à cet emplacement.

## Téléchargement de l’Acrobat

Pour que cette démonstration fonctionne sur votre système, vous devez créer un dossier appelé `acroforms` dans AEM Assets. Transférez l’Acroform dans ce dossier `acroforms`.

>[!NOTE]
>
>L’exemple de code recherche l’acroform dans ce dossier. L’acroformulaire est nécessaire pour fusionner les données envoyées du formulaire adaptatif.
