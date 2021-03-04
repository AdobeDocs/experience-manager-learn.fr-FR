---
title: Créer des modèles de processus AEM Forms réutilisables.
seo-title: Créer des modèles de processus AEM Forms réutilisables.
description: modèles de processus indépendants de Forms adaptatif.
seo-description: Modèles de processus indépendants de Forms adaptatif.
feature: workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---


# Créer des modèles de processus AEM Forms réutilisables{#create-re-usable-aem-forms-workflow-models}

A compter de la version AEM Forms 6.5, nous pouvons désormais créer des modèles de processus qui ne sont pas liés à un formulaire adaptatif spécifique. Grâce à cette fonctionnalité, vous pouvez désormais créer un modèle de processus qui peut être appelé sur différents envois de formulaires adaptatifs. Grâce à cette fonctionnalité, vous pouvez disposer d’un processus générique pour gérer toutes les soumissions de formulaires adaptatifs à des fins de révision et d’approbation.

Pour concevoir un tel flux de travail, effectuez les étapes suivantes :

1. Connexion à AEM
1. Pointez votre navigateur sur [modèle de flux de travaux](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html).
1. Cliquez sur Créer | Créer un modèle pour Ajouter un modèle de processus
1. Indiquez le nom et le titre appropriés au modèle de processus, puis cliquez sur Terminé.
1. Ouvrez le nouveau modèle en mode d’édition.
1. Faites glisser et déposez le composant Affecter une Tâche sur votre modèle de processus.
1. Ouvrez les propriétés de configuration du composant Attribuer une Tâche.
1. Onglet de l’onglet Forms et Documents
1. Sélectionnez Type - Formulaire adaptatif ou Formulaire adaptatif en lecture seule.

Il existe 3 manières de spécifier le chemin du formulaire

1. Disponible à un chemin absolu : cela signifie que le processus sera étroitement associé au formulaire adaptatif. Ce n&#39;est pas ce que nous voulons ici.
1. **Soumis au processus**  : cela signifie que lorsque le formulaire adaptatif est envoyé, le moteur de workflow extrait le nom du formulaire à partir des données envoyées. Il s’agit de l’option qui doit être sélectionnée.
1. Disponible à un chemin dans une variable : cela signifie que le formulaire adaptatif sera récupéré dans la variable de flux de travail.
La capture d’écran suivante vous montre l’option appropriée que vous devez choisir pour le processus de découplage du formulaire adaptatif.

![workflow, modèle](assets/workflomodel.PNG)