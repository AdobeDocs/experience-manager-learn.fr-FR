---
title: Boîte de réception AEM
description: Personnaliser la boîte de réception en ajoutant de nouvelles colonnes en fonction des données de workflow
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 3e1d86ab-e0c4-45d4-b998-75a44a7e4a3f
last-substantial-update: 2020-07-07T00:00:00Z
duration: 63
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 100%

---

# Boîte de réception AEM

La boîte de réception AEM rassemble les notifications et les tâches provenant de divers composants d’AEM, y compris les workflows de Forms. Lorsqu’un workflow de Forms contenant une étape Affecter une tâche est déclenché, l’application associée est répertoriée comme une tâche dans la boîte de réception de la personne désignée.

L’interface utilisateur de la boîte de réception fournit la liste et les vues de calendrier pour afficher les tâches. Vous pouvez également configurer les paramètres d’affichage. Vous pouvez filtrer les tâches en fonction de divers paramètres.

Vous pouvez personnaliser une boîte de réception Experience Manager en modifiant le titre par défaut d’une colonne, en réorganisant la position et en affichant des colonnes supplémentaires en fonction des données d’un workflow.

>[!NOTE]
>
>Vous devez faire partie de l’administration ou de l’administration de workflow pour personnaliser les colonnes de la boîte de réception.

## Personnaliser des colonnes

[Lancer la boîte de réception AEM](http://localhost:4502/aem/inbox)
Ouvrez le contrôle d’administration en cliquant sur l’icône _vue Liste_, puis en sélectionnant _Contrôle d’administration_ comme illustré dans la capture d’écran ci-dessous.

![admin-control](assets/open-customization.png)

Dans l’interface utilisateur de personnalisation des colonnes, vous pouvez effectuer les opérations suivantes :

* Supprimer des colonnes
* Réorganiser les colonnes
* Renommer des colonnes

## Personnaliser l’image de marque

Dans la personnalisation de l’image de marque, vous pouvez effectuer les opérations suivantes :

* ajouter le logo de votre organisation ;
* personnaliser le texte d’en-tête ;
* personnaliser le lien d’aide ;
* masquer les options de navigation.

![inbox-branding](assets/branding-customization.PNG)

## Étapes suivantes

[Ajouter une colonne Marié(e)](./add-married-column.md)
