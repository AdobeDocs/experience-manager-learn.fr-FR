---
title: Créer Forms pour la signature
description: Créez des formulaires qui doivent être inclus dans le pack de signature.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 1%

---


# Création de formulaires pour la signature

L’étape suivante consiste à créer les formulaires adaptatifs que vous souhaitez inclure dans le package. N’oubliez pas de respecter les points suivants lors de la création de formulaires à signer :

* Assurez-vous que les formulaires sont basés sur le modèle **SignMultipleForms**. Ainsi, les formulaires sont prérenseignés avec les données extraites de la base de données.

* Les formulaires doivent être configurés pour utiliser Adobe Sign et le champ signataire1 doit être associé au champ Courriel du client.
* Les formulaires doivent également être associés à clientLib appelé **getnextform**.
* Les formulaires doivent utiliser le composant Signature Step.
* Le formulaire doit également utiliser le composant personnalisé **Signer plusieurs formulaires**. Ce composant vous permet de naviguer jusqu’au formulaire suivant pour vous connecter au package.
* L’envoi du formulaire doit être configuré pour déclencher AEM processus **Mettre à jour l’état de la signature**
* Assurez-vous que le chemin d’accès au fichier de données est défini sur **Data.xml**. Cela est très important car l’exemple de code recherche un fichier appelé Data.xml dans la charge utile du processus d’envoi du formulaire.

Une fois votre formulaire créé, incluez les **champs_virgules** fragments de formulaire adaptatif dans le formulaire. Le fragment sera marqué comme masqué. Ce fragment contient les champs suivants.

* **signé**  : champ contenant l&#39;état de la signature
* **guid**  - Identificateur unique permettant d’identifier le formulaire du package
* **customerEmail**  : ce champ contient le courrier électronique du client.



>[!NOTE]
>Si vous souhaitez transférer des données d’un formulaire à un autre dans le package, veillez à ce que les champs du formulaire soient nommés de manière identique dans tous les formulaires.

## Tout le formulaire terminé

Une fois que tous les formulaires du package sont remplis et signés, nous devons afficher le message approprié. Ce message s’affiche à l’aide de la fonction Alldone. Le formulaire Autorisation est inclus dans les exemples de formulaires.

## Assets

Les exemples de formulaires, y compris ceux utilisés dans ce didacticiel, peuvent être [téléchargés ici](assets/forms-for-signing.zip)
