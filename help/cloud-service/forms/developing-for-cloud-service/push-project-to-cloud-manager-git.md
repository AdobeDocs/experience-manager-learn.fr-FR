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
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# Push AEM project to cloud manager git repo

Au cours de l’étape précédente, nous avons synchronisé notre projet AEM avec le Forms adaptatif et les thèmes créés dans l’instance AEM.
Nous devons maintenant ajouter ces modifications à notre référentiel git local, puis envoyer le référentiel git local vers le référentiel git de cloud manager.
Ouvrez une invite de commande et accédez à c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

Les nouveaux fichiers sont alors ajoutés à la branche intermédiaire du référentiel git local.

```
git commit -m "My First AF"
```

Cela valide les fichiers à la branche principale de notre référentiel git local.

```
git push -f bankingapp master:"My First AF"
```

Dans la commande ci-dessus, nous poussons notre branche principale de notre référentiel git local vers la branche My First AF du référentiel cloud manager identifiée par le nom convivial de l’application bancaire.



