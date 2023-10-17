---
title: Envoyer le projet AEM vers le référentiel Cloud Manager
description: Envoyer le référentiel Git local vers le référentiel Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: ht
source-wordcount: '147'
ht-degree: 100%

---

# Envoyer le projet AEM vers le référentiel Git Cloud Manager

Au cours de l’étape précédente, nous avons synchronisé notre projet AEM avec les formulaires adaptatifs et les thèmes créés dans l’instance AEM.
Nous devons maintenant ajouter ces modifications à notre référentiel Git local, puis envoyer le référentiel Git local vers le référentiel Git Cloud Manager.
Ouvrez une invite de commande et accédez à c:\cloudmanager\aem-banking-app.
Exécutez les commandes suivantes :

```
git add .
```

Les nouveaux fichiers sont alors ajoutés à la branche d’évaluation du référentiel Git local.

```
git commit -m "My First AF"
```

Cela valide les fichiers à la branche principale de notre référentiel Git local.

```
git push -f bankingapp master:"MyFirstAF"
```

Dans la commande ci-dessus, nous envoyons la branche principale de notre référentiel Git local vers la branche MyFirstAF du référentiel Cloud Manager identifié par le nom convivial de l’application bancaire.

## Étapes suivantes

[Déployer le projet dans l’environnement de développement](./deploy-to-dev-environment.md)
