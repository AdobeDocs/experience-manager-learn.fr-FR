---
title: Génération de documents de canal d’impression à l’aide du dossier de contrôle
seo-title: Génération de documents de canal d’impression à l’aide du dossier de contrôle
description: Il s’agit de la partie 10 du tutoriel en plusieurs étapes pour créer votre premier document de communication interactive pour le canal d’impression. Dans cette partie, nous allons générer des documents du canal d’impression à l’aide du mécanisme du dossier de contrôle.
seo-description: Il s’agit de la partie 10 du tutoriel en plusieurs étapes pour créer votre premier document de communication interactive pour le canal d’impression. Dans cette partie, nous allons générer des documents du canal d’impression à l’aide du mécanisme du dossier de contrôle.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 1%

---


# Génération de documents de canal d’impression à l’aide du dossier de contrôle

Dans cette partie, nous allons générer des documents du canal d’impression à l’aide du mécanisme du dossier de contrôle.

Après avoir créé et testé votre document de canal d’impression, nous avons besoin d’un mécanisme pour générer ces documents en mode batch ou à la demande. En règle générale, ces types de documents sont générés en mode batch et le mécanisme le plus courant consiste à utiliser le dossier de contrôle.

Lorsque vous configurez un dossier de contrôle dans AEM, vous associez un script ECMA ou un code Java qui est exécuté lorsqu’un fichier est déposé dans le dossier de contrôle. Dans cet article, nous allons nous concentrer sur le script ECMA qui génère des documents de canal d’impression et les enregistre dans le système de fichiers.

La configuration du dossier de contrôle et le script ECMA font partie des ressources que vous avez importées au [début de ce tutoriel](introduction.md)

Le fichier d’entrée déposé dans le dossier de contrôle possède la structure suivante. Le script ECMA lit les numéros de compte et génère un document de canal d’impression pour chacun de ces comptes.

Pour plus d’informations sur le script ECMA pour la génération de documents, [reportez-vous à cet article](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

Pour générer un document du canal d’impression à l’aide du mécanisme du dossier de contrôle, procédez comme suit :

* [Suivez les étapes mentionnées dans ce document.](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Connectez-vous à crx et accédez à /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Assurez-vous que le chemin d’accès à interactiveCommunicationsDocument pointe vers le document correct que vous souhaitez imprimer.( Ligne 1)
* Notez le saveLocation (Line 2). Vous pouvez le modifier selon vos besoins.
* Assurez-vous que le paramètre d’entrée du modèle de données de formulaire est lié à l’attribut de requête et que sa valeur de liaison est définie sur &quot;accountnumber&quot;. Reportez-vous à la capture d’écran ci-dessous.
   ![request](assets/requestattributeprintchannel.gif)

* Créez le fichier accountnuméros.xml avec le contenu suivant

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

* Déposez le fichier xml dans C:\RenderPrintChannel\input

* Vérifiez les fichiers pdf à l’emplacement d’enregistrement spécifié dans le script ECMA.




