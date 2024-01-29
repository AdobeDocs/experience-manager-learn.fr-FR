---
title: AcroForms avec AEM Forms
description: Partie 3 d’un tutoriel qui intègre des AcroForms à AEM Forms. Testez le workflow et le formulaire adaptatif sur votre système.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 56
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '228'
ht-degree: 100%

---


# Tester cette fonctionnalité sur votre système

[Téléchargez et importez ce package dans AEM.](assets/acro-form-aem-form.zip)
Ce package contient l’exemple de workflow et la page html qui vous permet de créer le schéma à partir de l’AcroForm chargé.

## Configurer le workflow

1. [Ouvrez le modèle de workflow en mode d’édition](http://localhost:4502/editor.html/conf/global/settings/workflow/models/MergeAcroformData.html).
2. Ouvrez les propriétés de configuration de l’étape MergeAcrobatData.
3. Cliquez sur l’onglet Processus.
4. Assurez-vous que les arguments que vous transmettez correspondent à un dossier valide sur votre serveur.
5. Enregistrez les modifications.

## Créer un formulaire adaptatif

1. Créez un formulaire adaptatif à l’aide du schéma créé à l’étape précédente.
2. Faites glisser et déposez quelques éléments de schéma dans le formulaire adaptatif.
3. Configurez l’action Envoyer du formulaire adaptatif à envoyer au workflow AEM (MergeAcrobatData).
4. **Veillez à spécifier le chemin d’accès au fichier de données sur « Data.xml ». Cette étape est très importante, car l’exemple de code recherche un fichier appelé Data.xml dans la payload du workflow.**
5. Prévisualisez le formulaire adaptatif, remplissez le formulaire et envoyez-le.
6. Vous devriez voir le PDF avec les données fusionnées enregistrées dans le dossier spécifié à l’étape 4 sous le workflow de configuration.

>[!NOTE]
>
>Le fichier PDF généré par la fusion des données avec l’AcroForm est enregistré sous la forme pdfdocument.pdf dans le dossier de payload du workflow. Ce document peut ensuite être utilisé pour un traitement ultérieur dans le cadre du workflow.
