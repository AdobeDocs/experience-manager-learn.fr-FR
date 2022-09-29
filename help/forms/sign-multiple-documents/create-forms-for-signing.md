---
title: Création de Forms pour la signature
description: Créez des formulaires qui doivent être inclus dans le package de signature.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Créer des formulaires à signer

L’étape suivante consiste à créer les formulaires adaptatifs que vous souhaitez inclure dans le module. N’oubliez pas de respecter les points suivants lors de la création de formulaires à signer :

* Assurez-vous que les formulaires reposent sur la variable **SignMultipleForms** modèle. Ainsi, les formulaires sont préremplis avec les données récupérées dans la base de données.

* Les formulaires doivent être configurés pour utiliser Adobe Sign et le champ signer1 doit être associé au champ Email du client.
* Les formulaires doivent également être associés à clientLib appelé **getnextform**
* Les formulaires doivent utiliser le composant Étape de signature.
* Le formulaire doit également utiliser la variable **Signature de plusieurs formulaires** composant. Ce composant vous permet d’accéder au formulaire suivant pour vous connecter au module.
* L’envoi du formulaire doit être configuré pour déclencher AEM workflow **Mettre à jour le statut de la signature**
* Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Ceci est très important, car l’exemple de code recherche un fichier appelé Data.xml dans la payload du processus d’envoi du formulaire.

Une fois le formulaire créé, incluez la variable **commonfields** fragment de formulaire adaptatif dans le formulaire. Le fragment est marqué comme masqué. Ce fragment contient les champs suivants.

* **signed** - Le champ contenant l’état de la signature
* **guid** - Identifiant unique permettant d’identifier le formulaire dans le module
* **customerEmail** - Ce champ contient l’adresse électronique du client.



>[!NOTE]
>Si vous souhaitez porter les données d’un formulaire vers un autre dans le package, veillez à ce que les champs du formulaire soient nommés de manière identique dans tous les formulaires.

## Tout le formulaire terminé

Une fois que tous les formulaires du package sont remplis et signés, nous devons afficher le message approprié. Ce message s’affiche à l’aide d’un formulaire Tout fait. Le formulaire Autorisé est inclus dans les exemples de formulaires.

## Assets

Les exemples de formulaires, y compris ceux utilisés dans ce tutoriel, peuvent être [téléchargé ici](assets/forms-for-signing.zip)
