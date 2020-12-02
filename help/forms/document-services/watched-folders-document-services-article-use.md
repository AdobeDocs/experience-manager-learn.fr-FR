---
title: Utilisation de dossiers de contrôle en AEM Forms
seo-title: Utilisation de dossiers de contrôle en AEM Forms
description: Configuration et utilisation des dossiers de contrôle en AEM Forms
seo-description: Configuration et utilisation des dossiers de contrôle en AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 20%

---


# Utilisation de dossiers de contrôle en AEM Forms{#using-watched-folders-in-aem-forms}

Un administrateur peut configurer un dossier réseau, appelé dossier de contrôle (en anglais Watched Folder), de sorte que lorsqu’un utilisateur y place un fichier (par exemple un fichier PDF), un flux de travail, un service ou une opération d’exécution de script démarre pour le traitement du fichier ajouté. Après que le service a effectué l’opération spécifiée, il enregistre le fichier obtenu dans un dossier de sortie spécifié. Pour plus d’informations sur le flux de travaux, le service et le script.

Pour en savoir plus sur la création d’un dossier de contrôle, [cliquez ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Les dossiers de contrôle sont utilisés pour générer des documents en mode batch. Grâce au mécanisme de dossier de contrôle, vous pouvez générer des communications interactives pour le canal d’impression ou utiliser le service de sortie pour fusionner des données avec le modèle.

Cet article porte sur le cas d’utilisation de la fusion de données avec un modèle utilisant le service de sortie via le mécanisme du dossier de contrôle.

Le service Output est un service OSGi qui fait partie d’AEM Document Services. Le service Output prend en charge divers formats de sortie et fonctions de conception de sortie d’AEM Forms Designer. Le service Output peut convertir les modèles XFA et les données XML pour générer des documents d’impression dans différents formats.

Pour en savoir plus sur le service de sortie, [veuillez cliquer ici](https://helpx.adobe.com/aem-forms/6/output-service.html).
Pour configurer le dossier de contrôle sur votre système, procédez comme suit :
* [Téléchargez et extrayez le contenu du fichier](assets/outputservicewatchedfolderkt.zip) zip. Ce fichier zip contient le package permettant de créer le dossier de contrôle et des fichiers d’exemple pour tester le service de sortie à l’aide du mécanisme du dossier de contrôle.
   * Système Windows

      * Importez le fichier outputservicewatchedfolder.zip dans AEM à l’aide du gestionnaire de packages.
      * Vous créez ainsi un dossier de contrôle nommé outputservicewatchedfolder sur votre lecteur C.
   * Système non Windows
      * [Ouvrez le paramètre de configuration du dossier de contrôle.](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Définissez la propriété de chemin d&#39;accès au dossier du noeud outservice pour qu&#39;il pointe vers un emplacement approprié.
      * Enregistrez vos modifications
      * L’emplacement mentionné ci-dessus sera votre dossier de contrôle.

Déposez les dossiers SamplePdfFile et SampleXdpFile dans le dossier input du dossier de contrôle. Une fois les fichiers traités, les résultats sont placés dans le dossier des résultats de votre dossier de contrôle.


>[!NOTE]
>
>Si le script associé au dossier de contrôle nécessite plusieurs fichiers, vous devez créer un dossier et y placer tous les fichiers requis, puis déposer le dossier dans le dossier input du dossier de contrôle.
>
>Si le script associé au dossier de contrôle ne nécessite qu’un seul fichier d’entrée, vous pouvez le déposer directement dans le dossier d’entrée de votre dossier de contrôle.

