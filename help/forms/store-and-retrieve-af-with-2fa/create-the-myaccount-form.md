---
title: Création de MyAccountForm
description: Créez le formulaire myaccount pour récupérer le formulaire partiellement rempli lors de la vérification réussie de l’ID de l’application et du numéro de téléphone.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Création de MyAccountForm

Le formulaire **MyAccountForm** est utilisé pour récupérer le formulaire adaptatif partiellement rempli une fois que l’utilisateur a vérifié l’ID de l’application et le numéro de mobile associé à l’ID de l’application.

![mon formulaire de compte](assets/6599.JPG)

Lorsque l’utilisateur saisit l’ID de l’application et clique sur la variable **FetchApplication** , le numéro de mobile associé à l’ID de l’application est récupéré de la base de données à l’aide de l’opération Get du modèle de données de formulaire.

Ce formulaire utilise l’appel POST du modèle de données de formulaire pour vérifier le numéro de mobile à l’aide d’OTP. L’action d’envoi du formulaire est déclenchée lors de la vérification réussie du numéro de mobile à l’aide du code suivant. Nous déclenchons l’événement click du bouton d’envoi nommé **submitForm**.

>[!NOTE]
> Vous devrez fournir la clé API et les valeurs secrètes de l’API propres à votre [Nexmo](https://dashboard.nexmo.com/) dans les champs appropriés de MyAccountForm ;

![trigger-submit](assets/trigger-submit.JPG)



Ce formulaire est associé à une action d’envoi personnalisée qui transfère l’envoi du formulaire vers le servlet monté sur **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Le code de la servlet monté sur **/bin/renderaf** transfère la demande de rendu du formulaire adaptatif storeafwithPièces jointes prérempli avec les données enregistrées.


* Le MyAccountForm peut être [téléchargé ici](assets/my-account-form.zip)

* Les exemples de formulaires reposent sur [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* [Gestionnaire d’envoi personnalisé](assets/custom-submit-my-account-form.zip) associés à l’envoi MyAccountForm doivent être importés dans AEM.

## Étapes suivantes

[Tester la solution en déployant les exemples de ressources](./deploy-this-sample.md)
