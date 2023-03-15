---
title: Rapport sur les champs de données de formulaire envoyés à l’aide d’Adobe Analytics
description: Intégration d’AEM Forms CS à Adobe Analytics pour créer des rapports sur les champs de données de formulaire
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Tester votre solution

Prévisualisez et envoyez votre formulaire à l’aide de plusieurs combinaisons de valeurs de formulaire. Prévoyez plusieurs à 30 minutes pour afficher vos données dans les rapports Adobe Analytics. Les données définies sur des props s’affichent dans les rapports plus tôt que les données définies sur des eVars.

## Suite de rapports

Les données de formulaire capturées dans Adobe Analytics sont présentées au format en anneau.

**Envois par État**

![applicantsbystate](assets/donut.png)

Erreurs de validation de champ

![field-validation-error](assets/donut-field-validation.png)

## Débogage

Assurez-vous que le formulaire adaptatif utilise le même conteneur de configuration que celui qui contient la configuration de lancement de l’Adobe.

Pour confirmer que le formulaire envoie des données à Adobe Analytics, procédez comme suit :

* Ouvrez les outils de développement dans votre navigateur.
* Saisissez dans le texte suivant dans le panneau Console.

```javascript
_satellite.setDebug(true)
```

Interagissez avec votre formulaire tout en gardant la fenêtre de console ouverte. Vous devriez voir quelque chose comme ça.

![console-debug](assets/debug.png)

## Utilisation de Adobe Experience Platform Debugger

Ajoutez la variable [Extension de débogueur AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) à votre navigateur (vous devez vous connecter) pour obtenir plus d’informations de débogage.

![platform-debugger](assets/platform-debugger.png)





