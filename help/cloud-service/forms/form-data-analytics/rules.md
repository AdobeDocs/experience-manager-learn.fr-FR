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
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 55
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '221'
ht-degree: 100%

---

# Définir la règle

Dans la propriété Balises, nous avons créé 2 nouvelles [règles](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html?lang=fr) (**Erreur de validation du champ et Envoi de formulaire**).

![adaptive-form](assets/rules.png)


## Erreur de validation du champ

La règle **Erreur de validation du champ** est déclenchée chaque fois qu’une erreur de validation se produit dans le champ du formulaire adaptatif. Par exemple, dans notre formulaire, si le numéro de téléphone ou l’e-mail n’est pas au format attendu, un message d’erreur de validation s’affiche.

La règle Erreur de validation du champ est configurée en définissant l’événement sur _**Adobe Experience Manager Forms-Error**_ comme illustré dans la copie d’écran.



![applicant-state-residence](assets/field_validation_error_rule.png)

La définition des variables Adobe Analytics est configurée comme suit :

![set action](assets/field_validation_action_rule.png)

## Règle d’envoi de formulaire

La règle Envoi de formulaire est déclenchée chaque fois qu’un formulaire adaptatif est envoyé avec succès.

La règle Envoi de formulaire est configurée à l’aide de l’événement _**Adobe Experience Manager Forms - Submit**_.

![form-submit-rule](assets/form-submit-rule.png)

Dans la règle Envoi de formulaire, la valeur de l’élément de données _**ApplicantsStateOfResidence**_ est mappée sur prop5 et celle de l’élément de données FormTitle sur prop8.

La définition des variables Adobe Analytics est configurée comme suit :
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

Lorsque vous êtes prêt ou prête à tester votre code de balises, [publiez les modifications apportées aux balises](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html?lang=fr) à l’aide du flux de publication.

## Étapes suivantes

[Tester la solution](./test.md)