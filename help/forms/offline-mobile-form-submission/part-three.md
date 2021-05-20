---
title: Déclencher AEM processus lors de l’envoi de formulaire HTM5
seo-title: Déclencher AEM processus lors de l’envoi d’un formulaire HTML5
description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
seo-description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# Processus de révision et d’approbation du PDF envoyé

La dernière et dernière étape consiste à créer AEM workflow qui génère un PDF statique ou non interactif en vue de sa révision et de son approbation. Le workflow sera déclenché par un lanceur AEM configuré sur le noeud `/content/pdfsubmissions`.

La capture d’écran suivante montre les étapes du workflow.

![flux de travail](assets/workflow.PNG)

## Étape de flux de travail Generate Non Interactive PDF

Le modèle XDP et les données à fusionner avec le modèle sont spécifiés ici. Les données à fusionner sont les données envoyées du PDF. Ces données envoyées sont stockées sous le noeud `/content/pdfsubmissions`.

![flux de travail](assets/generate-pdf1.PNG)

Le PDF généré est affecté à la variable de workflow `submittedPDF`.

![flux de travail](assets/generate-pdf2.PNG)

### Attribuer le fichier PDF généré à des fins de révision et d’approbation

Le composant de processus Affecter une tâche est utilisé ici pour affecter le fichier PDF généré à des fins de révision et d’approbation. La variable `submittedPDF` est utilisée dans l’onglet Forms et documents du composant de processus Assign Task.

![flux de travail](assets/assign-task.PNG)
