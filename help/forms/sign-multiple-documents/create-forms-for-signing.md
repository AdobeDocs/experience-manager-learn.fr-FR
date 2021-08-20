---
title: Création de Forms pour la signature
description: Créez des formulaires qui doivent être inclus dans le package de signature.
feature: Formulaires adaptatifs
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Développement
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 1%

---


# Créer des formulaires à signer

L’étape suivante consiste à créer les formulaires adaptatifs que vous souhaitez inclure dans le module. N’oubliez pas de respecter les points suivants lors de la création de formulaires à signer :

* Assurez-vous que les formulaires sont basés sur le modèle **SignMultipleForms** . Ainsi, les formulaires sont préremplis avec les données récupérées dans la base de données.

* Les formulaires doivent être configurés pour utiliser Adobe Sign et le champ signer1 doit être associé au champ Email du client.
* Les formulaires doivent également être associés à clientLib appelé **getnextform**
* Les formulaires doivent utiliser le composant Étape de signature.
* Le formulaire doit également utiliser le composant personnalisé **Sign Multiple Form**. Ce composant vous permet d’accéder au formulaire suivant pour vous connecter au module.
* L’envoi du formulaire doit être configuré pour déclencher AEM workflow **Mettre à jour le statut de la signature**
* Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Ceci est très important, car l’exemple de code recherche un fichier appelé Data.xml dans la payload du processus d’envoi du formulaire.

Une fois que vous avez créé votre formulaire, incluez le fragment de formulaire adaptatif **commonfields** dans le formulaire. Le fragment sera marqué comme masqué. Ce fragment contient les champs suivants.

* **signed**  : champ destiné à contenir l’état de la signature.
* **guid**  : identifiant unique permettant d’identifier le formulaire dans le module.
* **customerEmail**  : ce champ contient l’email du client.



>[!NOTE]
>Si vous souhaitez porter les données d’un formulaire vers un autre dans le package, veillez à ce que les champs du formulaire soient nommés de manière identique dans tous les formulaires.

## Tout le formulaire terminé

Une fois que tous les formulaires du package sont remplis et signés, nous devons afficher le message approprié. Ce message s’affiche à l’aide d’un formulaire Tout fait. Le formulaire Autorisé est inclus dans les exemples de formulaires.

## Ressources

Les exemples de formulaires, y compris ceux utilisés dans ce tutoriel, peuvent être [téléchargés ici](assets/forms-for-signing.zip)
