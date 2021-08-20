---
title: Utilisation de dossiers de contrôle dans AEM Forms
description: Configuration et utilisation des dossiers de contrôle dans AEM Forms
feature: Service Output
version: 6.4,6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 22%

---


# Utilisation de dossiers de contrôle dans AEM Forms{#using-watched-folders-in-aem-forms}

Un administrateur peut configurer un dossier réseau, appelé dossier de contrôle (en anglais Watched Folder), de sorte que lorsqu’un utilisateur y place un fichier (par exemple un fichier PDF), un flux de travail, un service ou une opération d’exécution de script démarre pour le traitement du fichier ajouté. Après que le service a effectué l’opération spécifiée, il enregistre le fichier obtenu dans un dossier de sortie spécifié. Pour plus d’informations sur le workflow, le service et le script.

Pour en savoir plus sur la création d’un dossier de contrôle, [cliquez ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Les dossiers de contrôle sont utilisés pour générer des documents en mode batch. Grâce au mécanisme du dossier de contrôle, vous pouvez générer des communications interactives pour le canal d’impression ou utiliser le service de sortie pour fusionner les données avec le modèle.

Cet article couvre le cas pratique de la fusion des données avec un modèle utilisant le service de sortie via le mécanisme du dossier de contrôle.

Le service Output est un service OSGi qui fait partie d’AEM Document Services. Le service Output prend en charge divers formats de sortie et fonctions de conception de sortie d’AEM Forms Designer. Le service Output peut convertir les modèles XFA et les données XML pour générer des documents d’impression dans différents formats.

Pour en savoir plus sur le service de sortie, [veuillez cliquer ici](https://helpx.adobe.com/aem-forms/6/output-service.html).
Pour configurer le dossier de contrôle sur votre système, procédez comme suit :
* [Téléchargez et extrayez le contenu du fichier zip](assets/outputservicewatchedfolderkt.zip). Ce fichier zip contient le package pour créer le dossier de contrôle et des fichiers d’exemple afin de tester le service de sortie à l’aide du mécanisme du dossier de contrôle.
   * Système Windows

      * Importez outputservicewatchedfolder.zip dans AEM à l’aide du gestionnaire de packages.
      * Cela crée un dossier de contrôle appelé outputservicewatchedfolder sur votre lecteur C.
   * Système non Windows
      * [Ouvrez le paramètre de configuration du dossier de contrôle.](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Définissez la propriété folder path du noeud outservice pour qu’elle pointe vers un emplacement approprié.
      * Enregistrez vos modifications
      * L’emplacement mentionné ci-dessus sera votre dossier de contrôle.

Déposez les dossiers SamplePdfFile et SampleXdpFile dans le dossier input du dossier de contrôle. Une fois le traitement des fichiers réussi, les résultats sont placés dans le dossier des résultats de votre dossier de contrôle.


>[!NOTE]
>
>Si le script associé au dossier de contrôle nécessite plusieurs fichiers, vous devez créer un dossier, y placer tous les fichiers requis et déposer le dossier dans le dossier input de votre dossier de contrôle.
>
>Si le script associé au dossier de contrôle ne nécessite qu’un seul fichier d’entrée, vous pouvez le déposer directement dans le dossier input de votre dossier de contrôle.

