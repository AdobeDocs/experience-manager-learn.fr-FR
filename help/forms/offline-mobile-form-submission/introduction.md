---
title: Déclencher AEM processus lors de l’envoi de formulaire HTM5
seo-title: Déclencher AEM processus lors de l’envoi d’un formulaire HTML5
description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
seo-description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# Téléchargement d’un formulaire mobile partiellement terminé et envoi au workflow AEM

Un cas d’utilisation courant consiste à avoir la possibilité de rendre le XDP au format HTML pour les activités de capture de données. Cela fonctionne bien lorsque les formulaires sont simples et peuvent être remplis et envoyés en ligne. Cependant, si le formulaire est complexe et que les utilisateurs peuvent ne pas être en mesure de le remplir en ligne, nous devons permettre aux utilisateurs de télécharger la version interactive du formulaire à remplir en utilisant Acrobat/Reader hors ligne. Une fois le formulaire renseigné, l’utilisateur peut se connecter pour envoyer le formulaire.
Pour réaliser ce cas d’utilisation, procédez comme suit :

* Possibilité de générer un PDF interactif/à remplir avec les données saisies dans le formulaire mobile
* Gérer l’envoi du PDF depuis Acrobat/Reader
* Processus Trigger Adobe Experience Manager (AEM) pour réviser le PDF envoyé

Ce tutoriel décrit les étapes nécessaires à la réalisation du cas d’utilisation ci-dessus. Vous trouverez [des exemples de code et de ressources liés à ce tutoriel ici.](part-four.md)

La vidéo suivante présente un aperçu du cas d’utilisation

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

