---
title: Préremplir un formulaire adaptatif basé sur des composants principaux
description: Découvrez comment préremplir un formulaire adaptatif avec des données.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 28
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '119'
ht-degree: 100%

---

# Tester la solution

Une fois le code déployé, créez un formulaire adaptatif basé sur les composants principaux. Associez le formulaire adaptatif au service de préremplissage comme illustré dans la copie d’écran ci-dessous.
![prefill-service](assets/pre-fill-service.png)

Chaque fois que le rendu du formulaire est effectué, le service de préremplissage associé est exécuté et les données renvoyées par le service de préremplissage sont renseignées dans le formulaire.

Par exemple, pour préremplir le formulaire avec les données associées au guid **d815a2b3-5f4c-4422-8197-d0b73479bf0e**, l’URL suivante est utilisée.
Le code du service de préremplissage extrait la valeur du paramètre guid et récupère les données associées au guid de la source de données.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
