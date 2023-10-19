---
title: Application React avec AEM Forms et Acrobat Sign
description: Acrobat Sign et AEM Forms permettent d’automatiser des transactions complexes et d’inclure des signatures électroniques légales dans le cadre d’une expérience numérique transparente.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '174'
ht-degree: 100%

---

# AEM Forms avec formulaire web Acrobat Sign


Ce tutoriel vous guide tout au long du cas d’utilisation qui consiste à générer un document de communication interactive avec les données envoyées à partir de l’application [React](https://react.dev/) et à présenter le document généré pour la signature à l’aide du formulaire web Acrobat Sign.

Voici le flux du cas d’utilisation :

* L’utilisateur ou l’utilisatrice remplit un formulaire dans l’application React.
* Les données de formulaire sont envoyées à un point d’entrée AEM Forms pour générer un document de communication interactive.
* Créez l’URL du widget Acrobat Sign à l’aide du document généré.
* Présentez l’URL du widget à l’application appelante pour que l’utilisateur ou l’utilisatrice puisse signer le document.

## Conditions préalables

Pour que le cas d’utilisation fonctionne, vous aurez besoin des éléments suivants :

* un serveur AEM avec package de module complémentaire Forms ;
* une [clé d’intégration pour une application Acrobat Sign](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html).

## Étapes suivantes

Écrivez un [service OSGi personnalisé pour générer un document de communication interactive](./create-ic-document.md) à l’aide d’une API documentée.
