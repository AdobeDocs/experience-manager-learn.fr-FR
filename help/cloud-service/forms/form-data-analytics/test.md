---
title: Créer un rapport sur les champs de données de formulaire envoyés à l’aide d’Adobe Analytics
description: Intégrer AEM Forms CS à Adobe Analytics pour créer des rapports sur les champs de données de formulaire
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 100%

---

# Tester votre solution

Prévisualisez et envoyez votre formulaire en utilisant plusieurs combinaisons de valeurs de formulaire. L’affichage de vos données dans les rapports Adobe Analytics peut prendre jusqu’à 30 minutes. Les données définies sur des props s’affichent plus rapidement dans les rapports que leurs homologues eVars.

## Suite de rapports

Les données de formulaire capturées dans Adobe Analytics sont présentées au format en anneau.

**Envois par État**

![applicantsbystate](assets/donut.png)

Erreurs de validation de champ

![field-validation-error](assets/donut-field-validation.png)

## Débogage

Assurez-vous que le formulaire adaptatif utilise le même conteneur de configuration que celui qui contient la configuration Adobe Launch.

Pour confirmer que le formulaire envoie des données à Adobe Analytics, procédez comme suit :

* Ouvrez les outils de développement du navigateur.
* Saisissez le texte suivant dans le panneau Console.

```javascript
_satellite.setDebug(true)
```

Interagissez avec votre formulaire tout en gardant la fenêtre de console ouverte. Vous devriez voir quelque chose comme ceci.

![console-debug](assets/debug.png)

## Utiliser Adobe Experience Platform Debugger

Ajoutez l’[extension AEP Debugger](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html?lang=fr) à votre navigateur (vous devez vous connecter) pour obtenir plus d’informations de débogage.

![platform-debugger](assets/platform-debugger.png)

## Félicitations.

Vous avez correctement intégré AEM Forms as a Cloud Service à Adobe Analytics pour créer des rapports sur les champs de données de formulaire.