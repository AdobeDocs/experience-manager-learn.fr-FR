---
title: Intégration d’AEM Forms à SendGrid
description: Tirez parti de la plateforme de diffusion email basée sur le cloud SengGrid en utilisant AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
kt: 13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 1%

---

# Intégration d’AEM Forms à SendGrid

Bienvenue dans ce guide technique, où nous allons explorer le processus d&#39;envoi d&#39;emails avec des modèles dynamiques SendGrid à partir d&#39;AEM Forms. Ce guide a pour but de vous donner une compréhension claire de la manière d’exploiter des modèles dynamiques pour personnaliser efficacement votre contenu d’email.

Les modèles dynamiques vous permettent de créer des modèles d’email qui peuvent afficher différents contenus aux destinataires en fonction des données capturées dans le formulaire adaptatif. En utilisant des variables de personnalisation, vous pouvez diffuser des expériences email ciblées et personnalisées qui résonnent auprès de votre audience.

De plus, nous allons nous familiariser avec l&#39;utilisation du fichier Swagger, qui vous permet de personnaliser davantage vos emails en incluant le nom et l&#39;adresse email du client, ainsi que de sélectionner le modèle d&#39;email dynamique approprié.

Suivez les instructions détaillées de ce document pour exploiter la puissance des modèles dynamiques SendGrid et d’AEM Forms, et augmenter vos communications par e-mail jusqu’à de nouveaux niveaux d’engagement et de pertinence. Commençons !

## Conditions préalables

Avant de procéder à l’envoi d’emails à l’aide de modèles dynamiques SendGrid à partir d’AEM Forms, vérifiez que vous avez satisfait aux conditions préalables suivantes :

1. **Compte SendGrid**: Inscrivez-vous à un compte SendGrid à l’adresse [https://sendgrid.com](https://sendgrid.com) pour accéder à leurs services de diffusion email. Vous aurez besoin des informations d’identification du compte pour intégrer SendGrid à AEM Forms.
1. **Familiarisation avec la création de sources de données**: posséder des connaissances opérationnelles de la création de sources de données dans AEM Forms. Si nécessaire, reportez-vous à la documentation relative à la [création de sources de données](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) pour obtenir des instructions détaillées.
1. **Familiarité avec le modèle de données de formulaire**: Comprendre le concept du modèle de données de formulaire dans AEM Forms. Si nécessaire, passez en revue la documentation relative à [création de modèles de données de formulaire](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=fr) pour vous assurer d’avoir la compréhension nécessaire.

En remplissant ces conditions préalables, vous disposez des connaissances et des ressources essentielles pour envoyer efficacement des emails à l’aide de modèles dynamiques SendGrid à partir d’AEM Forms.

## Exemples de ressources

Les exemples de ressources fournis avec cet article sont les suivants :

* **[Fichier Swagger](assets/SendGridWithDynamicTemplate.yaml)**: Ce fichier permet d&#39;envoyer des emails à l&#39;aide d&#39;un modèle d&#39;email dynamique. Il fournit les spécifications et configurations nécessaires à l’intégration à SendGrid et AEM Forms pour une diffusion email transparente.

N’hésitez pas à utiliser le fichier Swagger fourni comme référence ou point de départ pour la mise en oeuvre de la fonctionnalité d’email avec des modèles dynamiques.

## Instructions de test

Pour tester les fonctionnalités décrites dans ce guide, procédez comme suit :

1. Téléchargez la [fichier swagger](assets/SendGridWithDynamicTemplate.yaml) fourni dans le dossier assets.
2. Créez une source de données Restful à l’aide du fichier swagger téléchargé et de vos informations d’identification SendGrid.
3. Créez un modèle de données de formulaire basé sur la source de données Redémarrer.
4. Appeler la variable `mail/send` Fonctionnement POST du modèle de données de formulaire selon vos besoins. Par exemple, vous pouvez déclencher l’email en cliquant sur le bouton ou l’inclure dans votre workflow AEM Forms.

L’exemple de charge utile pour le service est le suivant. Remplacez les valeurs d’espace réservé par vos propres données :

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

Assurez-vous que la variable `template_id` correspond à l’identifiant de votre modèle d’email dynamique SendGrid, et les adresses email sont valides et vérifiées par SendGrid. Les valeurs de la variable `personalizations` vous permet de personnaliser l’email à l’aide des données saisies par l’utilisateur à partir du formulaire adaptatif.

En suivant ces étapes et en personnalisant la payload fournie, vous pouvez tester efficacement l’intégration des modèles dynamiques SendGrid avec AEM Forms.

