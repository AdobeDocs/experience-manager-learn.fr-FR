---
title: Déclencher un workflow AEM lors de l’introduction de l’envoi de formulaire HTM5
description: Continuez à remplir le formulaire mobile en mode hors ligne et soumettez-le pour déclencher le workflow AEM.
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
duration: 351
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '205'
ht-degree: 100%

---

# Télécharger un formulaire mobile partiellement terminé et l’envoyer au workflow AEM

Un cas d’utilisation courant consiste à avoir la possibilité d’effectuer le rendu du fichier XDP en tant que HTML pour les activités de capture de données. Cela fonctionne bien lorsque les formulaires sont simples et peuvent être remplis et envoyés en ligne. Cependant, le formulaire peut être complexe et les utilisateurs et utilisatrices peuvent ne pas être en mesure de le remplir en ligne. Nous devons permettre aux utilisateurs et aux utilisatrices de télécharger la version interactive du formulaire à remplir en utilisant Acrobat/Reader hors ligne. Une fois le formulaire rempli, l’utilisateur ou l’utilisatrice peut se connecter pour envoyer le formulaire.
Pour ce cas d’utilisation, procédez comme suit :

* Possibilité de générer un PDF interactif/à renseigner avec les données saisies dans le formulaire mobile
* Gérer l’envoi du PDF depuis Acrobat/Reader
* Déclencher le workflow Adobe Experience Manager (AEM) pour examiner le PDF envoyé

Ce tutoriel décrit les étapes relatives au cas d’utilisation ci-dessus. Des exemples de code et de ressources associés à ce tutoriel sont [disponibles ici.](part-four.md)

La vidéo suivante présente une vue d’ensemble du cas d’utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
