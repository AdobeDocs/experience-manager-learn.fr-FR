---
title: Déclencher le workflow AEM lors de l’envoi du formulaire HTML5 - Révision et approbation du PDF
description: Workflow de révision du PDF envoyé
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ec60d017-8b29-4185-a097-d809e18df4a7
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '172'
ht-degree: 100%

---

# Workflow de révision et d’approbation du PDF envoyé

La toute dernière étape consiste à créer un workflow AEM qui génère un PDF statique, ou non interactif, à des fins de révision et d’approbation. Le workflow est déclenché par un lanceur AEM configuré sur le nœud `/content/formsubmissions`.

La capture d’écran suivante montre les étapes impliquées dans ce workflow.

![workflow](assets/workflow.PNG)

## Étape de workflow Générer un PDF non interactif

Le modèle XDP et les données à fusionner avec le modèle sont spécifiés ici. Les données à fusionner sont les données envoyées du PDF. Ces données envoyées sont stockées sous le nœud ```/content/formsubmissions```.

![workflow](assets/generate-pdf1.PNG)

Le PDF généré est affecté à la variable de workflow appelée `submittedPDF`.

![workflow](assets/generate-pdf2.PNG)

### Attribuer le fichier PDF généré à des fins de révision et d’approbation

Le composant de workflow Affecter une tâche est utilisé ici pour affecter le PDF généré à des fins de révision et d’approbation. La variable `submittedPDF` est utilisée dans l’onglet Formulaires et documents du composant de workflow Affecter une tâche.

![workflow](assets/assign-task.PNG)


## Étapes suivantes

[Déploiement des ressources dans votre environnement](./deploy-assets.md)
