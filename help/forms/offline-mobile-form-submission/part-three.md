---
title: Déclencher le processus AEM sur l’envoi de formulaires HTML5
seo-title: Déclencher le flux de travail AEM sur l’envoi de formulaire HTML5
description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
seo-description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 4%

---


# Processus de révision et d’approbation du PDF envoyé

La dernière et dernière étape consiste à créer un processus AEM qui génère un PDF statique ou non interactif pour révision et approbation. Le flux de travail sera déclenché par un lanceur AEM configuré sur le noeud `/content/pdfsubmissions`.

La capture d’écran suivante montre les étapes du processus.

![flux de travail](assets/workflow.PNG)

## Procédure de génération de PDF non interactifs

Le modèle XDP et les données à fusionner avec le modèle sont spécifiés ici. Les données à fusionner sont les données envoyées à partir du PDF. Ces données envoyées sont stockées sous le noeud `/content/pdfsubmissions`.

![flux de travail](assets/generate-pdf1.PNG)

Le PDF généré est affecté à la variable de flux de travail `submittedPDF`.

![flux de travail](assets/generate-pdf2.PNG)

### Affecter le fichier pdf généré pour révision et approbation

Attribuer un composant de processus de tâche est utilisé ici pour affecter le PDF généré à des fins de révision et d’approbation. La variable `submittedPDF` est utilisée dans l’onglet Forms et Documents du composant de flux de travail Attribuer la Tâche.

![flux de travail](assets/assign-task.PNG)
