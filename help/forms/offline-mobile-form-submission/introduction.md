---
title: Déclencher AEM processus sur l’introduction de l’envoi de formulaire HTM5
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Téléchargement d’un formulaire mobile partiellement terminé et envoi au workflow AEM

Un cas d’utilisation courant consiste à avoir la possibilité de rendre le fichier XDP comme HTML pour les activités de capture de données. Cela fonctionne bien lorsque les formulaires sont simples et peuvent être remplis et envoyés en ligne. Cependant, si le formulaire est complexe et que les utilisateurs peuvent ne pas être en mesure de le remplir en ligne, nous devons permettre aux utilisateurs de télécharger la version interactive du formulaire à remplir en utilisant Acrobat/Reader hors ligne. Une fois le formulaire renseigné, l’utilisateur peut se connecter pour envoyer le formulaire.
Pour réaliser ce cas d’utilisation, procédez comme suit :

* Possibilité de générer un PDF interactif/à renseigner avec les données saisies dans le formulaire mobile
* Gérer l’envoi du PDF depuis Acrobat/Reader
* Déclencher le workflow Adobe Experience Manager (AEM) pour examiner le PDF envoyé

Ce tutoriel décrit les étapes nécessaires à la réalisation du cas d’utilisation ci-dessus. Voici des exemples de code et des ressources liés à ce tutoriel : [disponible ici.](part-four.md)

La vidéo suivante présente un aperçu du cas d’utilisation

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
