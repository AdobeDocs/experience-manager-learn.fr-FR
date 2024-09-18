---
title: Déclencher le processus AEM sur l’envoi de formulaire HTML5 - Réviser et approuver le PDF
description: Workflow de révision du PDF envoyé
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 5f42678502a785ead29982044d1f3f5ecf023e0f
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 69%

---

# Workflow de révision et d’approbation du PDF envoyé

La dernière et dernière étape consiste à créer AEM workflow qui génère un PDF statique ou non interactif pour révision et approbation. Le workflow est déclenché par un lanceur AEM configuré sur le nœud `/content/formsubmissions`.

La capture d’écran suivante montre les étapes impliquées dans ce workflow.

![workflow](assets/workflow.PNG)

## Étape de workflow Générer un PDF non interactif

Le modèle XDP et les données à fusionner avec le modèle sont spécifiés ici. Les données à fusionner sont les données envoyées du PDF. Ces données envoyées sont stockées sous le noeud ```/content/formsubmissions```

![workflow](assets/generate-pdf1.PNG)

Le PDF généré est affecté à la variable de workflow appelée `submittedPDF`.

![workflow](assets/generate-pdf2.PNG)

### Attribuer le fichier PDF généré à des fins de révision et d’approbation

Le composant de workflow Affecter une tâche est utilisé ici pour affecter le PDF généré à des fins de révision et d’approbation. La variable `submittedPDF` est utilisée dans l’onglet Formulaires et documents du composant de workflow Affecter une tâche.

![workflow](assets/assign-task.PNG)


## Étapes suivantes

[Déployer les ressources dans votre environnement](./deploy-assets.md)