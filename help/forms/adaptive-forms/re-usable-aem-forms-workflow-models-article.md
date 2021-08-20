---
title: Créez des modèles de workflow AEM Forms réutilisables.
description: modèles de workflow indépendants de Forms adaptatif.
feature: Workflow
version: 6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# Création de modèles de processus AEM Forms réutilisables{#create-re-usable-aem-forms-workflow-models}

À compter de la version 6.5 d’AEM Forms, nous pouvons désormais créer des modèles de workflow qui ne sont pas liés à un formulaire adaptatif spécifique. Grâce à cette fonctionnalité, vous pouvez désormais créer un modèle de processus qui peut être appelé sur différents envois de formulaire adaptatif. Grâce à cette fonctionnalité, vous pouvez disposer d’un processus générique pour traiter tous les envois de formulaire adaptatif à des fins de révision et d’approbation.

Pour concevoir un tel workflow, procédez comme suit :

1. Connexion à AEM
1. Pointez votre navigateur vers le [modèle de workflow](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. Cliquez sur Créer | Créer un modèle pour ajouter un modèle de processus
1. Indiquez le nom et le titre appropriés au modèle de processus, puis cliquez sur Terminé
1. Ouvrez le modèle nouvellement créé en mode d’édition.
1. Faites glisser et déposez le composant Affecter une tâche sur votre modèle de workflow.
1. Ouvrez les propriétés de configuration du composant Assign Task
1. Onglet Forms et documents
1. Sélectionnez le Type : formulaire adaptatif ou formulaire adaptatif en lecture seule.

Le chemin du formulaire peut être spécifié de trois façons :

1. Disponible à un chemin absolu : cela signifie que le processus sera étroitement couplé à un formulaire adaptatif. Ce n&#39;est pas ce que nous voulons ici.
1. **Envoyé au processus**  : cela signifie que lorsque le formulaire adaptatif est envoyé, le moteur de processus extrait le nom du formulaire des données envoyées. Il s’agit de l’option qui doit être sélectionnée.
1. Disponible dans un chemin d’accès dans une variable : cela signifie que le formulaire adaptatif sera récupéré dans la variable de workflow.
La capture d’écran suivante montre l’option appropriée que vous devez choisir pour le processus de découplage du formulaire adaptatif

![workflowmodel](assets/workflomodel.PNG)