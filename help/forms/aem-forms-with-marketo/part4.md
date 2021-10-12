---
title: AEM Forms avec Marketo (partie 4)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 020852f16de0cdb1e17e19ad989dabf37b7f61f5
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 3%

---

# Création d’un formulaire adaptatif à l’aide du modèle de données de formulaire

L’étape suivante consiste à créer un formulaire adaptatif et à le baser sur le modèle de données de formulaire créé à l’étape précédente.
L’utilisateur saisit l’ID de piste et lorsqu’il sort du service Marketo pour obtenir les pistes par identifiant, il est appelé. Les résultats de l’opération de service sont ensuite associés aux champs appropriés de la Forms adaptative.

1. Créez un formulaire adaptatif et basez-le sur &quot;Modèle de formulaire vierge&quot;, associez-le au modèle de données de formulaire créé à l’étape précédente.
1. Ouvrez le formulaire en mode de modification
1. Faites glisser et déposez un composant TextField et un composant Panneau sur le formulaire adaptatif. Définissez le titre du composant TextField &quot;Enter Lead Id&quot; et définissez son nom sur &quot;LeadId&quot;.
1. Faites glisser et déposez 2 composants TextField sur le composant Panneau .
1. Définissez le nom et le titre des 2 composants de champ de texte sur FirstName et LastName .
1. Configurez le composant Panneau pour qu’il soit un composant répétable en définissant les valeurs Minimum à 1 et Maximum à -1. Cela est nécessaire, car le service Marketo renvoie un tableau d’objets de piste et vous devez disposer d’un composant répétable pour afficher les résultats. Cependant, dans ce cas, nous ne récupérerons qu’un seul objet de piste, car nous effectuons des recherches sur les objets de piste à l’aide de son identifiant.
1. Créez une règle sur le champ LeadId comme illustré dans l’image ci-dessous.
1. Prévisualisez le formulaire, saisissez un ID de piste valide dans le champ LeadID et désélectionnez-le. Les champs Prénom et Nom doivent être renseignés avec les résultats de l’appel de service.

La capture d’écran suivante explique les paramètres de l’éditeur de règles

![ruleeditor](assets/ruleeditor.jfif)

## Débogage

Si vous utilisez les bundles fournis avec cet article, vous pouvez activer [debug logs](http://localhost:4502/system/console/slinglog) pour les classes suivantes :

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
