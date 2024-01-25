---
title: Intégrer AEM Forms à SendGrid
description: Tirez parti de la plateforme de diffusion d’e-mail basée sur le cloud SengGrid à l’aide d’AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 129
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 100%

---

# Intégrer AEM Forms à SendGrid

Bienvenue dans ce guide technique où nous allons explorer le processus d’envoi d’e-mails avec des modèles dynamiques SendGrid à partir d’AEM Forms. Ce guide a pour but de vous faire comprendre clairement comment exploiter des modèles dynamiques pour personnaliser efficacement le contenu de vos e-mails.

Les modèles dynamiques vous permettent de créer des modèles d’e-mail pouvant afficher différents contenus aux personnes destinataires en fonction des données capturées dans le formulaire adaptatif. En utilisant des variables de personnalisation, vous pouvez diffuser des expériences d’e-mail ciblées et personnalisées qui résonnent auprès de votre audience.

De plus, nous allons nous familiariser avec l’utilisation du fichier Swagger, qui vous permet de personnaliser davantage vos e-mails en incluant le nom et l’adresse e-mail du client ou de la cliente, ainsi que de sélectionner le modèle d’e-mail dynamique approprié.

Suivez les instructions détaillées de ce document pour exploiter la puissance des modèles dynamiques SendGrid et d’AEM Forms et augmenter les niveaux d’engagement et de pertinence de vos communications par e-mail. Commençons.

## Conditions préalables

Avant de procéder à l’envoi d’e-mails à l’aide de modèles dynamiques SendGrid à partir d’AEM Forms, vérifiez que vous satisfaisez les conditions préalables suivantes :

1. **Compte SendGrid** : ouvrez un compte SendGrid à l’adresse [https://sendgrid.com](https://sendgrid.com) pour accéder à leurs services de diffusion d’e-mail. Vous aurez besoin des informations d’identification du compte pour intégrer SendGrid à AEM Forms.
1. **Connaissances relatives à la création de sources de données** : vous devez disposer de connaissances opérationnelles sur la création de sources de données dans AEM Forms. Si nécessaire, reportez-vous à la documentation relative à la [création de sources de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=fr) pour obtenir des instructions détaillées.
1. **Connaissances relatives au modèle de données de formulaire** : vous devez comprendre le concept du modèle de données de formulaire dans AEM Forms. Si nécessaire, passez en revue la documentation relative à la [création de modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr) pour vous assurer d’avoir les connaissances nécessaires.

En satisfaisant ces conditions préalables, vous disposez des connaissances et des ressources essentielles pour envoyer efficacement des e-mails à l’aide des modèles dynamiques SendGrid à partir d’AEM Forms.

## Exemples de ressources

Les exemples de ressources fournis avec cet article sont les suivants :

* **[Fichier Swagger](assets/SendGridWithDynamicTemplate.yaml)** : ce fichier permet d’envoyer des e-mails à l’aide d’un modèle d’e-mail dynamique. Il fournit les spécifications et configurations nécessaires à l’intégration de SendGrid à AEM Forms pour une diffusion d’e-mail aisée.

N’hésitez pas à utiliser le fichier Swagger fourni comme référence ou point de départ pour la mise en œuvre de la fonctionnalité d’e-mail avec des modèles dynamiques.

## Instructions de test

Pour tester la fonctionnalité décrite dans ce guide, procédez comme suit :

1. Téléchargez le [fichier Swagger](assets/SendGridWithDynamicTemplate.yaml) fourni dans le dossier des ressources.
2. Créez une source de données Restful à l’aide du fichier Swagger téléchargé et de vos informations d’identification SendGrid.
3. Créez un modèle de données de formulaire basé sur la source de données Restful.
4. Appelez l’opération POST `mail/send` du modèle de données de formulaire selon vos besoins. Par exemple, vous pouvez déclencher l’e-mail en cliquant sur le bouton ou l’inclure dans votre workflow AEM Forms.

L’exemple de payload pour le service se présente comme suit. Remplacez les valeurs d’espace réservé par vos propres données :

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

Assurez-vous que l’`template_id` correspond à l’identifiant de votre modèle d’e-mail dynamique SendGrid et que les adresses e-mail sont valides et vérifiées par SendGrid. Les valeurs de la section `personalizations` vous permettent de personnaliser l’e-mail à l’aide des données saisies par l’utilisateur ou l’utilisatrice à partir du formulaire adaptatif.

En suivant ces étapes et en personnalisant la payload fournie, vous pouvez tester efficacement l’intégration des modèles dynamiques SendGrid à AEM Forms.
