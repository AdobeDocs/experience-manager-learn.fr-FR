---
title: AEM Forms avec Marketo (partie 4)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 74
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 100%

---

# Créer un formulaire adaptatif à l’aide du modèle de données de formulaire

L’étape suivante consiste à créer un formulaire adaptatif et à le baser sur le modèle de données de formulaire créé à l’étape précédente.
La personne utilisatrice saisit l’ID du lead et il est appelé lorsqu’il sort du service Marketo pour obtenir les leads par identifiant. Les résultats de l’opération de service sont ensuite mappés aux champs appropriés des formulaires adaptatifs.

1. Créez un formulaire adaptatif, basez-le sur « Modèle de formulaire vierge », et associez-le au modèle de données de formulaire créé à l’étape précédente.
1. Ouvrez le formulaire adaptatif en mode de modification.
1. Faites glisser et déposez un composant TextField et un composant Panel sur le formulaire adaptatif. Définissez le titre du composant TextField « Enter Lead Id » et définissez son nom sur « LeadId ».
1. Faites glisser et déposez 2 composants TextField sur le composant Panel.
1. Définissez le nom et le titre des 2 composants de TextField sur FirstName (Prénom) et LastName (Nom).
1. Configurez le composant Panel pour qu’il soit un composant répétable en définissant Minimum sur 1 et Maximum sur -1. Cela est nécessaire, car le service Marketo renvoie un tableau d’objets lead et vous devez disposer d’un composant répétable pour afficher les résultats. Cependant, dans ce cas, nous ne récupérons qu’un seul objet lead, car nous recherchons les objets lead selon leur ID.
1. Créez une règle sur le champ LeadId comme illustré dans l’image ci-dessous.
1. Prévisualisez le formulaire, saisissez un ID de lead valide dans le champ LeadID et quittez le formulaire. Les champs Prénom et Nom doivent être renseignés avec les résultats de l’appel de service.

La capture d’écran suivante explique les paramètres de l’éditeur de règles.

![ruleeditor](assets/ruleeditor.jfif)

## Débogage

Si vous utilisez les lots fournis avec cet article, vous pouvez activer les [journaux de débogage](http://localhost:4502/system/console/slinglog) pour les classes suivantes :

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## Félicitations.

Vous avez correctement intégré AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.