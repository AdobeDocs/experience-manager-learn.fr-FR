---
title: Création de MyAccountForm
description: Créez le formulaire myaccount pour récupérer le formulaire partiellement rempli lors de la vérification réussie de l’ID de l’application et du numéro de téléphone.
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Développement
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 1%

---



# Création de MyAccountForm

Le formulaire **MyAccountForm** est utilisé pour récupérer le formulaire adaptatif partiellement rempli après que l’utilisateur a vérifié l’ID de l’application et le numéro de mobile associé à l’ID de l’application.

![mon formulaire de compte](assets/6599.JPG)

Lorsque l’utilisateur saisit l’ID de l’application et clique sur le bouton **FetchApplication** , le numéro de mobile associé à l’ID de l’application est récupéré de la base de données à l’aide de l’opération Get du modèle de données de formulaire.

Ce formulaire utilise l’appel POST du modèle de données de formulaire pour vérifier le numéro de mobile à l’aide d’OTP. L’action d’envoi du formulaire est déclenchée lors de la vérification réussie du numéro de mobile à l’aide du code suivant. Nous déclenchons l’événement click du bouton submit nommé **submitForm**.

>[!NOTE]
> Vous devrez fournir la clé API et les valeurs du secret API propres à votre compte [Nexmo](https://dashboard.nexmo.com/) dans les champs appropriés de MyAccountForm.

![trigger-submit](assets/trigger-submit.JPG)



Ce formulaire est associé à une action d’envoi personnalisée qui transfère l’envoi du formulaire vers le servlet monté sur **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Le code du servlet monté sur **/bin/renderaf** transfère la demande de rendu du formulaire adaptatif storeafwithments prefill avec les données enregistrées.


* Le MyAccountForm peut être [téléchargé ici](assets/my-account-form.zip)

* Les exemples de formulaires sont basés sur [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires soient correctement rendus.

* [Le ](assets/custom-submit-my-account-form.zip) gestionnaire d’envoi personnalisé associé à l’envoi MyAccountForm doit être importé dans AEM.
