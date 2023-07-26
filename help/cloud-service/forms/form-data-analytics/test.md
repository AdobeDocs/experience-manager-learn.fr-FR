---
title: Rapport sur les champs de données de formulaire envoyés à l’aide d’Adobe Analytics
description: Intégration d’AEM Forms CS à Adobe Analytics pour créer des rapports sur les champs de données de formulaire
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
kt: 12557
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

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

## Utiliser l’Adobe Experience Platform Debugger

Ajoutez la variable [Extension de débogueur AEP](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html?lang=fr) à votre navigateur (vous devez vous connecter) pour obtenir plus d’informations de débogage.

![platform-debugger](assets/platform-debugger.png)

## Félicitations

Vous avez correctement intégré AEM Forms as a Cloud Service à Adobe Analytics pour créer des rapports sur les champs de données de formulaire.