---
title: Générer des documents de canal d’impression à l’aide du dossier de contrôle
description: Il s’agit de la partie 10 du tutoriel en plusieurs étapes sur la création de votre premier document de communication interactive pour le canal d’impression. Dans cette partie, nous allons générer des documents de canal d’impression à l’aide du mécanisme du dossier de contrôle.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 80
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 100%

---

# Générer des documents de canal d’impression à l’aide du dossier de contrôle

Dans cette partie, nous allons générer des documents de canal d’impression à l’aide du mécanisme du dossier de contrôle.

Après avoir créé et testé votre document de canal d’impression, nous avons besoin d’un mécanisme pour générer ces documents en mode batch ou à la demande. En règle générale, ces types de documents sont générés en mode batch et le mécanisme le plus courant consiste à utiliser le dossier de contrôle.

Lorsque vous configurez un dossier de contrôle dans AEM, vous associez un script ECMA ou un code Java qui est exécuté lorsqu’un fichier est déposé dans le dossier de contrôle. Dans cet article, nous allons nous concentrer sur le script ECMA qui génère des documents de canal d’impression et les enregistre dans le système de fichiers.

La configuration du dossier de contrôle et le script ECMA font partie des ressources que vous avez importées au [début de ce tutoriel](introduction.md).

Le fichier d’entrée déposé dans le dossier de contrôle possède la structure suivante. Le script ECMA lit les numéros de compte et génère un document de canal d’impression pour chacun de ces comptes.

Pour plus d’informations sur le script ECMA pour la génération de documents, [voir cet article](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md).

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

Pour générer un document de canal d’impression à l’aide du mécanisme du dossier de contrôle, procédez comme suit :

* [Suivez les étapes mentionnées dans ce document.](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Connectez-vous à crx et accédez à /etc/fd/watchfolder/scripts/PrintPDF.ecma.

* Assurez-vous que le chemin d’accès à interactiveCommunicationsDocument pointe vers le document correct que vous souhaitez imprimer.(Ligne 1)
* Prenez note du saveLocation (Ligne 2). Vous pouvez le modifier selon vos besoins.
* Assurez-vous que le paramètre d’entrée du modèle de données de formulaire est lié à l’attribut de requête et que sa valeur de liaison est définie sur « accountnumber ». Reportez-vous à la capture d’écran ci-dessous.
  ![Requête.](assets/requestattributeprintchannel.gif)

* Créez un fichier accountnumbers.xml avec le contenu suivant :

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* Déposez le fichier XML dans C:\RenderPrintChannel\input

* Vérifiez les fichiers PDF à l’emplacement d’enregistrement spécifié dans le script ECMA.

## Étapes suivantes

[Ouvrir l’interface utilisateur de l’agent lors de l’envoi du formulaire](./opening-agent-ui-on-form-submission.md)