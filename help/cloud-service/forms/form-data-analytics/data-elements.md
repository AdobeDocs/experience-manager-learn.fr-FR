---
title: Créer un rapport sur les champs de données de formulaire envoyés à l’aide d’Adobe Analytics
description: Intégrer AEM Forms CS à Adobe Analytics pour créer des rapports sur les champs de données de formulaire
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '138'
ht-degree: 100%

---

# Créer des éléments de données

Dans la propriété des balises, nous avons ajouté deux nouveaux éléments de données (ApplicantsStateOfResidence et validationError).

![adaptive-form](assets/data_elements.png)

## ApplicantStateOfResidence

L’élément de données **ApplicantStateOfResidence** a été configuré en sélectionnant **Core** dans la liste déroulante d’extension et **Code personnalisé** pour le type d’élément de données, comme illustré dans la capture d’écran ci-dessous.
![applicant-state-residence](assets/applicantstateofresidence.png)

Le code personnalisé suivant a été utilisé pour capturer la valeur du champ de formulaire adaptatif **_state_**.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

L’élément de données **ValidationError** a été configuré en sélectionnant **Core** dans la liste déroulante d’extension et **Code personnalisé** pour le type d’élément de données, comme illustré dans la capture d’écran ci-dessous.

![validation-error](assets/validation-error.png)

Le code personnalisé suivant a été écrit pour définir la valeur de l’élément de données `validationError`.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## Étapes suivantes

[Créer des règles](./rules.md)