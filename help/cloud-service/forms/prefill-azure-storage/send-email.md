---
title: Envoyer un e-mail à l’aide de SendGrid
description: Déclencher un e-mail avec un lien vers le formulaire enregistré
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-8474
duration: 25
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '117'
ht-degree: 100%

---

# Envoyer un e-mail

Une fois les données du formulaire enregistrées dans l’espace de stockage Azure Blob, un e-mail contenant un lien vers le formulaire enregistré est envoyé à l’utilisateur ou à l’utilisatrice. Cet e-mail est envoyé à l’aide de l’API REST SendGrid.

Retrouvez le fichier Swagger, le modèle de données de formulaire et la configuration des services cloud, nécessaires à l’envoi des e-mails, dans la section Ressources de l’article.

Vous devrez créer un compte SendGrid et un modèle dynamique afin d’ingérer les données capturées dans le formulaire adaptatif.


Voici le fragment de code HTML utilisé dans le modèle dynamique. La valeur du paramètre blobID est transmise à l’aide du service d’appel du modèle de données de formulaire.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


