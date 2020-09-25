---
title: Utilisation de AEM Forms avec Adobe Sign
description: adobe sign et AEM Forms permettent d'automatiser des transactions complexes et d'inclure des signatures électroniques légales dans le cadre d'une expérience numérique transparente.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 41%

---

# Utilisation de AEM Forms avec Adobe Sign

Adobe Sign autorise les processus de signature électronique pour les formulaires adaptatifs. Les signatures électroniques améliorent les processus de traitement des documents pour les services juridiques, commercial, des ressources humaines, et bien d’autres domaines.
L’intégration entre AEM Forms et Adobe Sign vous permet d’effectuer les opérations suivantes :

* Utilisation de Forms adaptatif pour capturer des données et présenter un Document d’enregistrement (DE) généré automatiquement pour les signatures
* Créez un Forms adaptatif basé sur votre modèle PDF. Fusionnez les données avec le modèle pdf et présentez-les pour les signatures.
* Envoi de documents pour signature à l’aide du composant de flux de travail Signer le Document

## Conditions préalables

Vous avez besoin de ce qui suit pour intégrer Adobe Sign à AEM Forms :

* Un serveur AEM Forms SSL
* Un compte de développeur Adobe Sign actif.
* Une application API Adobe Sign
* Informations d’identification (ID client et secret client) de l’application API Adobe Sign.

