---
title: Créer des formulaires à signer
description: Créez des formulaires à inclure dans le package de signature.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 98
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 100%

---

# Créer des formulaires à signer

L’étape suivante consiste à créer les formulaires adaptatifs que vous souhaitez inclure dans le package. Veillez à respecter les points suivants lors de la création de formulaires à signer :

* Assurez-vous que les formulaires reposent sur le modèle **SignMultipleForms**. Cela permet de s’assurer que les formulaires sont préremplis avec les données issues de la base de données.

* Les formulaires doivent être configurés pour utiliser Acrobat Sign et le champ signer1 doit être associé au champ E-mail du client ou de la cliente.
* Les formulaires doivent également être associés à la bibliothèque cliente **getnextform**.
* Les formulaires doivent utiliser le composant Étape de signature.
* Le formulaire doit également utiliser le composant **Signer plusieurs formulaires**. Ce composant vous permet d’accéder au formulaire suivant à signer dans le package.
* L’envoi du formulaire doit être configuré pour déclencher le workflow AEM **Mettre à jour le statut de la signature**.
* Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Cette étape est très importante, car l’exemple de code recherche un fichier appelé Data.xml dans la payload du processus d’envoi du formulaire.

Une fois le formulaire créé, incluez le fragment de formulaire adaptatif **commonfields** dans le formulaire. Le fragment est marqué comme masqué. Ce fragment contient les champs suivants.

* **signed** : le champ contenant le statut de la signature.
* **guid** : identifiant unique permettant d’identifier le formulaire dans le package.
* **customerEmail** : ce champ contient l’adresse e-mail du client ou de la cliente.



>[!NOTE]
>Si vous souhaitez transposer les données d’un formulaire à un autre dans le package, veillez à nommer les champs du formulaire de manière identique dans tous les formulaires.

## Formulaire « All Done » (Tous les champs sont remplis)

Une fois tous les formulaires du package complétés et signés, nous devons afficher un message approprié. Ce message s’affiche à l’aide du formulaire « Alldone » (Tous les champs sont remplis). Le formulaire « Alldone » est inclus dans les exemples de formulaires.

## Ressources

[Téléchargez ici](assets/forms-for-signing.zip) les exemples de formulaires, y compris ceux utilisés dans ce tutoriel.

## Étapes suivantes

[Tester la solution sur votre système local](./testing-and-trouble-shooting.md)
