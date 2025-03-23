---
title: AcroForms avec AEM Forms
description: Partie 2 de l’intégration d’AcroForms à AEM Forms. Créez un schéma à partir d’un AcroForm.
feature: adaptive-forms
doc-type: Tutorial
version: Experience Manager 6.5
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 34
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 100%

---


# Créer un schéma à partir de l’AcroForm

L’étape suivante consiste à créer un schéma à partir de l’AcroForm établi à l’étape précédente. Un exemple d’application est fourni pour créer le schéma dans le cadre de ce tutoriel. Pour créer le schéma, procédez comme suit :

1. Ouvrez une session dans [CRXDE Lite](http://localhost:4502/crx/de).
2. Ouvrez le fichier `/apps/AemFormsSamples/components/createxsd/POST.jsp`.
3. Modifiez l’`saveLocation` par un dossier approprié sur votre disque dur. Assurez-vous que le dossier d’enregistrement est déjà créé.
4. Pointez votre navigateur vers la page [Créer un schéma XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) hébergée sur AEM.
5. Faites glisser l’AcroForm et déposez-le.
6. Ouvrez le dossier spécifié à l’étape 3. Le fichier de schéma est enregistré à cet emplacement.

## Charger l’AcroForm

Pour que cette démonstration fonctionne sur votre système, vous devez créer un dossier appelé `acroforms` dans AEM Assets. Chargez l’AcroForm dans le dossier `acroforms`.

>[!NOTE]
>
>L’exemple de code recherche l’AcroForm dans ce dossier. L’AcroForm est nécessaire pour fusionner les données envoyées du formulaire adaptatif.
