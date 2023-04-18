---
title: Application React avec AEM Forms et Acrobat Sign
description: Acrobat Sign et AEM Forms permettent d’automatiser des transactions complexes et d’inclure des signatures électroniques légales dans le cadre d’une expérience numérique transparente.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---

# AEM Forms avec formulaire web Acrobat Sign


Ce tutoriel vous guide tout au long du cas d’utilisation de la génération d’un document de communication interactive avec les données envoyées à partir du [React](https://react.dev/) et présentant le document généré pour la signature à l’aide du formulaire web Acrobat Sign.

Voici le flux du cas d’utilisation :

* L’utilisateur remplit un formulaire dans l’application React.
* Les données de formulaire sont envoyées à un point de terminaison AEM Forms pour générer un document de communication interactive.
* Créez l’URL du widget Acrobat Sign à l’aide du document généré.
* Présenter l’URL du widget à l’application qui l’appelle pour que l’utilisateur puisse signer le document.

## Prérequis

Pour que le cas d’utilisation fonctionne, vous aurez besoin des éléments suivants :

* Un serveur AEM avec package de module complémentaire Forms
* Un [clé d’intégration pour une application Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

