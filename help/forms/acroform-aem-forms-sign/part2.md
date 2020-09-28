---
title: Acroformes avec AEM Forms
seo-title: Fusionner les données de formulaire adaptatif avec Acroform
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 1ba56ad44df4dc327cf37d39ac72539b5c7af4a2
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---


# Création d’un Schéma à partir de l’acroformulaire

L’étape suivante consiste à créer un schéma à partir de l’Acroform créé à l’étape précédente. Un exemple d’application est fourni pour créer le schéma dans le cadre de ce didacticiel. Pour créer le schéma, suivez les instructions suivantes :

1. Connexion au [CRXDE Lite](http://localhost:4502/crx/de)
2. Ouvrir dans le fichier `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Modifiez le dossier `saveLocation` en un dossier approprié sur votre disque dur. Assurez-vous que le dossier dans lequel vous enregistrez est déjà créé.
4. Pointez votre navigateur sur [Créer une page XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) hébergée sur AEM.
5. Faites glisser et déposez l’Acroform.
6. Vérifiez le dossier spécifié à l’étape 3. Le fichier de schéma est enregistré à cet emplacement.

## Téléchargement de l’Acroform

Pour que cette démonstration fonctionne sur votre système, vous devez créer un dossier appelé `acroforms` en AEM Assets. Téléchargez l’Acroform dans ce `acroforms` dossier.

>[!NOTE]
L’exemple de code recherche l’acroformulaire dans ce dossier. L’acroformulaire est nécessaire pour fusionner les données envoyées du formulaire adaptatif.
