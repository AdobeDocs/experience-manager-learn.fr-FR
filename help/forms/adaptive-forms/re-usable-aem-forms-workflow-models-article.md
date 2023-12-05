---
title: Modèles de worflow AEM Forms réutilisables
description: Découvrez comment créer des modèles de workflow indépendants des formulaires adaptatifs.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 83
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 100%

---

# Créer des modèles de workflow AEM Forms réutilisables{#create-re-usable-aem-forms-workflow-models}

À compter de la version 6.5 d’AEM Forms, il est désormais possible de créer des modèles de workflow qui ne sont pas liés à un formulaire adaptatif spécifique. Grâce à cette fonctionnalité, vous pouvez désormais créer un modèle de workflow qui peut être appelé sur différents envois de formulaire adaptatif. Grâce à cette fonctionnalité, vous pouvez disposer d’un workflow générique pour traiter tous les envois de formulaire adaptatif à des fins de révision et d’approbation.

Pour concevoir un tel workflow, procédez comme suit :

1. Connectez-vous à AEM.
1. Pointez votre navigateur sur le [modèle de workflow](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html).
1. Cliquez sur __Créer > Créer un modèle__ pour ajouter un modèle de workflow.
1. Indiquez le nom et le titre appropriés au modèle de workflow, puis cliquez sur Terminé.
1. Ouvrez le modèle nouvellement créé en mode d’édition.
1. Faites glisser et déposez le composant Attribuer une tâche sur votre modèle de workflow.
1. Ouvrez les propriétés de configuration du composant Attribuer une tâche.
1. Accédez à l’onglet Formulaires et documents.
1. Sélectionnez le Type : formulaire adaptatif ou formulaire adaptatif en lecture seule.

Le chemin d’accès au formulaire peut être spécifié de trois façons :

1. Disponible à un chemin d’accès absolu : cela signifie que le processus est étroitement lié au formulaire adaptatif. Ce n’est pas ce que nous voulons ici.
1. **Envoyé au workflow** : cela signifie que lorsque le formulaire adaptatif est envoyé, le moteur de workflow extrait le nom du formulaire des données envoyées. Il s’agit de l’option à sélectionner.
1. Disponible à un chemin d’accès dans une variable : cela signifie que le formulaire adaptatif est sélectionné dans la variable de workflow.
La copie d’écran suivante vous montre l’option correcte que vous devez choisir pour le découplage du workflow du formulaire adaptatif.

![Modèles de workflow AEM Forms réutilisables.](assets/workflomodel.PNG)
