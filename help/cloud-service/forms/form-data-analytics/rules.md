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
source-git-commit: 439167be96959baea54f50a221c6d26f8fab78b2
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Définir la règle

Dans la propriété Balises, nous avons créé 2 nouvelles [rules](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html)(**Erreur de validation du champ et FormSubmit**).
![formulaire adaptatif](assets/rules.png)


## Erreur de validation du champ

Le **Erreur de validation du champ** est déclenchée chaque fois qu’une erreur de validation se produit dans le champ de formulaire adaptatif. Par exemple, dans notre formulaire, si le numéro de téléphone ou l’email n’est pas au format attendu, un message d’erreur de validation s’affiche.
La règle d’erreur de validation de champ est configurée en définissant l’événement sur _**Adobe Experience Manager Forms-Error**_ comme illustré dans la capture d’écran

![candidat-state-résidence](assets/field_validation_error_rule.png)

Adobe Analytics - Set Variables est configuré comme suit :

![action de définition](assets/field_validation_action_rule.png)

## Règle d’envoi de formulaire

La règle d’envoi de formulaire est déclenchée chaque fois qu’un formulaire adaptatif est envoyé avec succès.
La règle d’envoi de formulaire est configurée à l’aide de la fonction _**Adobe Experience Manager Forms - Envoyer**_ event

![form-submit-rule](assets/form-submit-rule.png)

Dans la règle d’envoi de formulaire, la valeur de l’élément de données _**applicantStateOfRésidence**_ est mappé sur prop5 et la valeur de l’élément de données FormTitle est mappée sur prop8.

Les variables Adobe Analytics - Set sont configurées comme suit.
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Lorsque vous êtes prêt à tester votre code de balises,[publier les modifications apportées aux balises ;](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) à l’aide du flux de publication.
