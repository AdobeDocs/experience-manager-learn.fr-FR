---
title: Déclencher le workflow AEM lors de l’envoi du formulaire HTM5 - Réviser et approuver le PDF
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continuez à remplir le formulaire mobile en mode hors ligne et soumettez-le pour déclencher le workflow AEM.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '170'
ht-degree: 100%

---

# Workflow de révision et d’approbation du PDF envoyé

La toute dernière étape consiste à créer un workflow AEM qui génère un PDF statique, ou non interactif, à des fins de révision et d’approbation. Le workflow est déclenché par un lanceur AEM configuré sur le nœud `/content/pdfsubmissions`.

La capture d’écran suivante montre les étapes impliquées dans ce workflow.

![workflow](assets/workflow.PNG)

## Étape de workflow Générer un PDF non interactif

Le modèle XDP et les données à fusionner avec le modèle sont spécifiés ici. Les données à fusionner sont les données envoyées du PDF. Ces données envoyées sont stockées sous le nœud `/content/pdfsubmissions`.

![workflow](assets/generate-pdf1.PNG)

Le PDF généré est affecté à la variable de workflow appelée `submittedPDF`.

![workflow](assets/generate-pdf2.PNG)

### Attribuer le fichier PDF généré à des fins de révision et d’approbation

Le composant de workflow Affecter une tâche est utilisé ici pour affecter le PDF généré à des fins de révision et d’approbation. La variable `submittedPDF` est utilisée dans l’onglet Formulaires et documents du composant de workflow Affecter une tâche.

![workflow](assets/assign-task.PNG)
