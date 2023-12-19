---
title: Préremplir le formulaire adaptatif basé sur un composant principal
description: Découvrez comment préremplir un formulaire adaptatif avec des données
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
source-git-commit: d9612adbc2ff3e601c2efe5a779c03ad24769276
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 3%

---

# Tester la solution

Une fois le code déployé, créez un formulaire adaptatif basé sur les composants principaux. Associez le formulaire adaptatif au service de préremplissage comme illustré dans la capture d’écran ci-dessous.
![prefill-service](assets/pre-fill-service.png)

Chaque fois que le formulaire est généré, le service de préremplissage associé est exécuté et le formulaire est renseigné avec les données renvoyées par le service de préremplissage.

Par exemple, pour préremplir le formulaire avec les données associées au guid **d815a2b3-5f4c-4422-8197-d0b73479bf0e**, l’URL suivante est utilisée.
Le code du service de préremplissage extrait la valeur du paramètre guid et récupère les données associées au guid de la source de données.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
