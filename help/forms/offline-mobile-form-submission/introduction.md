---
title: Déclencher le processus AEM sur l’envoi de formulaires HTML5
seo-title: Déclencher le flux de travail AEM sur l’envoi de formulaire HTML5
description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
seo-description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# Téléchargement d’un formulaire mobile partiellement terminé et envoi au processus AEM

Un cas d’utilisation courant consiste à avoir la possibilité de rendre le XDP au format HTML pour les activités de capture de données. Cela fonctionne bien lorsque les formulaires sont simples et peuvent être remplis et envoyés en ligne. Cependant, si le formulaire est complexe et que les utilisateurs ne peuvent pas le remplir en ligne, nous devons permettre aux utilisateurs de télécharger une version interactive du formulaire à remplir en utilisant l’Acrobat/Reader hors ligne. Une fois le formulaire rempli, l’utilisateur peut se connecter pour envoyer le formulaire.
Pour ce faire, nous devons effectuer les étapes suivantes :

* Possibilité de générer un PDF interactif/remplissable avec les données saisies dans le formulaire pour périphériques mobiles
* Gérer l’envoi du PDF à partir de l’Acrobat/Reader
* Déclenchez le processus Adobe Experience Manager (AEM) pour réviser le PDF envoyé.

Ce didacticiel décrit les étapes nécessaires pour accomplir le cas d’utilisation ci-dessus. L&#39;exemple de code et les ressources associés à ce didacticiel sont [disponibles ici.](part-four.md)

La vidéo suivante présente un aperçu du cas d&#39;utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

