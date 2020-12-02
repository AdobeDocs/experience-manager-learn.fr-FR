---
title: Acroformes avec AEM Forms
seo-title: Fusionner les données de formulaire adaptatif avec Acroform
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 1%

---


# Testez cette fonctionnalité sur votre système

[Téléchargez et importez ce module dans ](assets/acro-form-aem-form.zip)
AEMTson module contient l’exemple de flux de travail et la page html qui vous permet de créer le schéma à partir de l’Acroform téléchargé.

## Configurer le processus

1. [Ouvrez le modèle de processus en mode](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html) d’édition.
2. Ouvrez les propriétés de configuration de l&#39;étape MergeAcroformData.
3. Cliquez sur l’onglet Processus.
4. Assurez-vous que les arguments que vous transmettez sont un dossier valide sur votre serveur.
5. Enregistrez les modifications.

## Créer un formulaire adaptatif

1. Créez un formulaire adaptatif à l’aide du schéma créé à l’étape précédente.
2. Faites glisser quelques éléments de schéma vers le formulaire adaptatif.
3. Configurez l’action d’envoi du formulaire adaptatif à envoyer au processus AEM (MergeAcroformData).
4. **Veillez à spécifier le chemin d’accès au fichier de données comme &quot;Data.xml&quot;. C&#39;est très important car l&#39;exemple de code recherche un fichier appelé Data.xml dans la charge utile du flux de travail.**
5. Prévisualisation du formulaire adaptatif, remplissez le formulaire et envoyez-le.
6. Le fichier PDF doit s’afficher avec les données fusionnées enregistrées dans le dossier spécifié à l’étape 4 sous le processus de configuration.

>[!NOTE]
>
>Le fichier pdf généré par la fusion des données avec l’acroformulaire est enregistré sous le nom pdfdocument.pdf dans le dossier de charge du flux de travail. Ce document peut ensuite être utilisé pour un traitement ultérieur dans le cadre du processus
