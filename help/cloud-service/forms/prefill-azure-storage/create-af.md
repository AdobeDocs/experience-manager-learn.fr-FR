---
title: Stocker les données de formulaire adaptatif dans le stockage Azure
description: Créer et configurer un formulaire adaptatif pour stocker des données dans le stockage Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
thumbnail: 335423.jpg
exl-id: 0b543c6b-9cfd-4fac-b8d0-33153c036f4b
duration: 73
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 100%

---

# Assemblage

Toutes les configurations/intégrations requises sont désormais disponibles pour le cas d’utilisation. La dernière étape consiste à créer un formulaire adaptatif basé sur le modèle de données de formulaire pris en charge par Azure Storage.

Créez un formulaire adaptatif et assurez-vous qu’il est basé sur le modèle de formulaire adaptatif approprié. Cela garantit que notre code personnalisé associé au modèle sera exécuté chaque fois qu’un formulaire adaptatif est rendu.

## Exemple de formulaire adaptatif

Dans le formulaire, nous avons ajouté 2 champs masqués.

* ID d’objet blob : ce champ est renseigné avec un GUID lorsque le champ est initialisé. La valeur de ce champ est utilisée comme ID d’objet blob pour identifier de manière unique le stockage blob des données de formulaire. Cet ID d’objet blob est utilisé pour identifier les données de formulaire.
* ID d’objet blob renvoyé : ce champ est renseigné avec le résultat de l’appel de service pour stocker des données dans Azure Storage. Cette valeur sera identique à l’ID d’objet blob de l’étape précédente.

Le formulaire comporte les règles de fonctionnement suivantes :

* Le bouton Enregistrer le formulaire s’affiche lorsque l’utilisateur ou l’utilisatrice saisit son adresse e-mail. En cliquant sur le bouton Enregistrer le formulaire, les données de formulaire sont stockées dans Azure Storage à l’aide de l’opération d’appel du service du modèle de données de formulaire.
* L’ID d’objet blob renvoyé par le service d’appel est stocké dans le champ ID d’objet blob. Lorsque cette valeur change, un e-mail est envoyé au demandeur ou à la demandeuse à l’aide de SendGrid. L’e-mail contient le lien permettant d’ouvrir le formulaire partiellement rempli identifié par l’ID d’objet blob.
* Un texte de confirmation s’affiche à l’utilisateur ou l’utilisatrice lorsque les données ont été stockées avec succès dans Azure Storage.

### Étapes suivantes

[Déployer des exemples de ressources](./deploy-sample-assets.md)
