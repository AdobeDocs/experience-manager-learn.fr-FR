---
title: Déclencher AEM processus sur l’envoi de formulaire HTM5 - Réviser et approuver le PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a767d8f8-d75e-4472-9139-c08d804ee076
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 2%

---

# Processus de révision et d’approbation du PDF envoyé

La dernière et dernière étape consiste à créer AEM workflow qui génère un PDF statique, ou non interactif, à des fins de révision et d’approbation. Le workflow sera déclenché par un lanceur AEM configuré sur le noeud . `/content/pdfsubmissions`.

La capture d’écran suivante montre les étapes du workflow.

![flux de travail](assets/workflow.PNG)

## Étape de workflow Générer un PDF non interactif

Le modèle XDP et les données à fusionner avec le modèle sont spécifiés ici. Les données à fusionner sont les données envoyées du PDF. Ces données envoyées sont stockées sous le noeud . `/content/pdfsubmissions`.

![flux de travail](assets/generate-pdf1.PNG)

Le PDF généré est affecté à la variable de workflow appelée `submittedPDF`.

![flux de travail](assets/generate-pdf2.PNG)

### Attribuer le fichier PDF généré à des fins de révision et d’approbation

Le composant de workflow Affecter une tâche est utilisé ici pour affecter le PDF généré à des fins de révision et d’approbation. La variable `submittedPDF` est utilisé dans l’onglet Forms et documents du composant de workflow Affecter une tâche .

![flux de travail](assets/assign-task.PNG)
