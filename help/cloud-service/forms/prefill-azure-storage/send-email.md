---
title: Envoyer un e-mail à l’aide de SendGrid
description: Déclencher un email avec un lien vers le formulaire enregistré
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 8474
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Envoyer un email

Une fois les données du formulaire enregistrées dans le stockage Azure Blob, un courrier électronique contenant un lien vers le formulaire enregistré est envoyé à l’utilisateur. Cet e-mail est envoyé à l’aide de l’API REST SendGrid.

Le fichier swagger, le modèle de données de formulaire et la configuration des services cloud nécessaires pour envoyer des emails vous sont fournis dans le cadre des ressources de l’article.

Vous devrez créer un compte SendGrid, un modèle dynamique qui peut ingérer les données capturées dans le formulaire adaptatif.


Voici le fragment de code HTML utilisé dans le modèle dynamique. La valeur du paramètre blobID est transmise à l’aide du service d’appel du modèle de données de formulaire.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


