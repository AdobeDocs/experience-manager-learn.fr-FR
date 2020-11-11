---
title: Création du formulaire MonCompte
description: Créez le formulaire myaccount pour récupérer le formulaire partiellement rempli lors de la vérification réussie de l'ID de demande et du numéro de téléphone.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# Création du formulaire MonCompte

Le formulaire **MyAccountForm** est utilisé pour récupérer le formulaire adaptatif partiellement rempli après que l’utilisateur a vérifié l’identifiant de l’application et le numéro de mobile associé à l’identifiant de l’application.

![mon formulaire de compte](assets/6599.JPG)

Lorsque l’utilisateur entre l’ID de l’application et clique sur le bouton **FetchApplication** , le numéro de mobile associé à l’ID d’application est extrait de la base de données à l’aide de l’opération Get du modèle de données de formulaire.

Ce formulaire utilise l’appel POST du modèle de données de formulaire pour vérifier le numéro de mobile à l’aide de la méthode OTP. L’action d’envoi du formulaire est déclenchée lors d’une vérification réussie du numéro de mobile à l’aide du code suivant. Nous déclenchons le événement de clics du bouton d’envoi nommé **submitForm**.

>[!NOTE]
> Vous devrez fournir la clé d&#39;API et les valeurs de secret d&#39;API propres à votre compte [Nexmo](https://dashboard.nexmo.com/) dans les champs appropriés de MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Ce formulaire est associé à une action d’envoi personnalisée qui transfère l’envoi du formulaire vers la servlet montée sur **/bin/renderaf.**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Le code de la servlet monté sur **/bin/renderaf** transfère la demande de rendu du stockage avec les pièces jointes du formulaire adaptatif prérempli avec les données enregistrées.


* Le formulaire MonCompte peut être [téléchargé ici](assets/my-account-form.zip)

* Les exemples de formulaires sont basés sur un modèle [de formulaire adaptatif](assets/custom-template-with-page-component.zip) personnalisé qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* [Le gestionnaire](assets/custom-submit-my-account-form.zip) d’envoi personnalisé associé à l’envoi de MyAccountForm doit être importé dans AEM.
