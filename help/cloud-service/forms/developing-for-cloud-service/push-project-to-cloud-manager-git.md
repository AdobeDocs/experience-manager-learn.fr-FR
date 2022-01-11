---
title: Push AEM project to cloud manager repository
description: Poussez le référentiel git local vers le référentiel cloud manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Push AEM project to cloud manager git repo

Au cours de l’étape précédente, nous avons synchronisé notre projet AEM avec le Forms adaptatif et les thèmes créés dans l’instance AEM.
Nous devons maintenant ajouter ces modifications à notre référentiel git local, puis envoyer le référentiel git local vers le référentiel git de cloud manager.
Ouvrez une invite de commande et accédez à c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .
```

Les nouveaux fichiers sont alors ajoutés à la branche intermédiaire du référentiel git local.

```
git commit -m "My First AF"
```

Cela valide les fichiers à la branche principale de notre référentiel git local.

```
git push -f bankingapp master:"MyFirstAF"
```

Dans la commande ci-dessus, nous mettons en oeuvre la branche principale de notre référentiel git local vers la branche MyFirstAF du référentiel de gestion de cloud identifié par le nom convivial de l’application bancaire.
