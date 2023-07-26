---
title: Acroforms avec AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Partie 3 d’un tutoriel qui intègre Acrobat à AEM Forms. Testez le processus et le formulaire adaptatif sur votre système.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 3%

---


# Tester cette fonctionnalité sur votre système

[Téléchargez et importez ce package dans AEM](assets/acro-form-aem-form.zip)
Ce package contient l’exemple de workflow et la page html qui vous permet de créer le schéma à partir de l’Acroform chargé.

## Configuration du processus

1. [Ouvrir le modèle de workflow en mode d’édition](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Ouvrez les propriétés de configuration de l’étape MergeAcrobatData .
3. Cliquez sur l’onglet Processus .
4. Assurez-vous que les arguments que vous transmettez sont un dossier valide sur votre serveur.
5. Enregistrez les modifications.

## Créer un formulaire adaptatif

1. Créez un formulaire adaptatif à l’aide du schéma créé à l’étape précédente.
2. Faites glisser et déposez quelques éléments de schéma dans le formulaire adaptatif.
3. Configurez l’action d’envoi du formulaire adaptatif à envoyer au processus d’AEM (MergeAcrobatData).
4. **Veillez à spécifier le chemin d’accès au fichier de données comme &quot;Data.xml&quot;. C’est très important, car l’exemple de code recherche un fichier appelé Data.xml dans la charge utile du workflow.**
5. Prévisualisez le formulaire adaptatif, remplissez le formulaire et envoyez-le.
6. Vous devriez voir le PDF avec les données fusionnées enregistrées dans le dossier spécifié à l’étape 4 sous le workflow de configuration.

>[!NOTE]
>
>Le fichier pdf généré par la fusion des données avec l’acroform est enregistré sous la forme pdfdocument.pdf dans le dossier de charge utile du workflow. Ce document peut ensuite être utilisé pour un traitement ultérieur dans le cadre du workflow
