---
title: Créer le formulaire MyAccountForm
description: Créez le formulaire MyAccountForm pour récupérer le formulaire partiellement rempli lors de la vérification réussie de l’ID de l’application et du numéro de téléphone.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 76
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 100%

---

# Créer le formulaire MyAccountForm

Le formulaire **MyAccountForm** est utilisé pour récupérer le formulaire adaptatif partiellement rempli une fois que l’utilisateur ou l’utilisatrice a vérifié l’ID de l’application et le numéro de téléphone portable associé à l’ID de l’application.

![Formulaire MyAccountForm.](assets/6599.JPG)

Lorsque l’utilisateur ou l’utilisatrice saisit l’ID de l’application et clique sur le bouton **FetchApplication**, le numéro de téléphone portable associé à l’ID de l’application est récupéré de la base de données à l’aide de l’opération GET du modèle de données de formulaire.

Ce formulaire utilise l’appel POST du modèle de données de formulaire pour vérifier le numéro de téléphone portable à l’aide d’OTP. L’action d’envoi du formulaire est déclenchée lors de la vérification réussie du numéro de téléphone portable à l’aide du code suivant. Nous déclenchons l’événement clic du bouton d’envoi nommé **submitForm**.

>[!NOTE]
> Vous devrez fournir la clé API et les valeurs secrètes de l’API propres à votre [Nexmo](https://dashboard.nexmo.com/) dans les champs appropriés du formulaire MyAccountForm.

![trigger-submit](assets/trigger-submit.JPG)



Ce formulaire est associé à une action d’envoi personnalisée qui transfère l’envoi du formulaire vers le servlet monté sur **/bin/renderaf**.

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Le code du servlet monté sur **/bin/renderaf** transfère la requête de rendu du formulaire adaptatif storeafwithattachments prérempli avec les données enregistrées.


* Le formulaire MyAccountForm peut être [téléchargé ici](assets/my-account-form.zip).

* Les exemples de formulaires reposent sur un [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires soient rendus correctement.

* Le [gestionnaire d’envoi personnalisé](assets/custom-submit-my-account-form.zip) associé à l’envoi du formulaire MyAccountForm doit être importé dans AEM.

## Étapes suivantes

[Tester la solution en déployant les exemples de ressources.](./deploy-this-sample.md)
