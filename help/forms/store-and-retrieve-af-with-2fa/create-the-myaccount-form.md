---
title: Création du formulaire MonCompte
description: Créez le formulaire myaccount pour récupérer le formulaire partiellement rempli lors de la vérification réussie de l'ID de demande et du numéro de téléphone.
feature: formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---



# Création du formulaire MonCompte

Le formulaire **MyAccountForm** est utilisé pour récupérer le formulaire adaptatif partiellement rempli après que l’utilisateur a vérifié l’identifiant de l’application et le numéro de téléphone mobile associé à l’identifiant de l’application.

![mon formulaire de compte](assets/6599.JPG)

Lorsque l’utilisateur entre l’ID d’application et clique sur le bouton **FetchApplication**, le numéro de téléphone mobile associé à l’ID d’application est extrait de la base de données à l’aide de l’opération Get du modèle de données de formulaire.

Ce formulaire utilise l’appel POST du modèle de données de formulaire pour vérifier le numéro de mobile à l’aide de la méthode OTP. L’action d’envoi du formulaire est déclenchée lors d’une vérification réussie du numéro de mobile à l’aide du code suivant. Nous déclenchons le événement de clic du bouton d’envoi nommé **submitForm**.

>[!NOTE]
> Vous devez fournir la clé d&#39;API et les valeurs de secret d&#39;API spécifiques à votre compte [Nexmo](https://dashboard.nexmo.com/) dans les champs appropriés de MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Ce formulaire est associé à une action d’envoi personnalisée qui transfère l’envoi du formulaire vers la servlet montée sur **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Le code de la servlet monté sur **/bin/renderaf** transfère la demande de rendu du stockage avec les pièces jointes du formulaire adaptatif prérempli avec les données enregistrées.


* Le MyAccountForm peut être [téléchargé ici](assets/my-account-form.zip)

* Les exemples de formulaires sont basés sur [modèle de formulaire adaptatif personnalisé](assets/custom-template-with-page-component.zip) qui doit être importé dans AEM pour que les exemples de formulaires s’affichent correctement.

* [Les gestionnaires d’envoi personnalisés ](assets/custom-submit-my-account-form.zip) associés à l’envoi de MyAccountForm doivent être importés dans AEM.
