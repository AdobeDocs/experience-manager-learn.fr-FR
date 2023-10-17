---
title: Utiliser les dossiers de contrôle dans AEM Forms
description: Configurer et utiliser les dossiers de contrôle dans AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '423'
ht-degree: 100%

---

# Utiliser les dossiers de contrôle dans AEM Forms{#using-watched-folders-in-aem-forms}

Un administrateur peut configurer un dossier réseau, appelé dossier de contrôle (en anglais Watched Folder), de sorte que lorsqu’un utilisateur y place un fichier (par exemple un fichier PDF), un workflow, un service ou une opération d’exécution de script démarre pour le traitement du fichier ajouté. Après que le service a effectué l’opération spécifiée, il enregistre le fichier obtenu dans un dossier de sortie spécifié. Pour plus d’informations sur le workflow, le service et le script.

Pour en savoir plus sur la création d’un dossier de contrôle, [cliquez ici](https://experienceleague.adobe.com/docs/experience-manager-64/forms/publish-process-aem-forms/creating-configure-watched-folder.html?lang=fr).

Les dossiers de contrôle sont utilisés pour générer des documents en mode batch. À l’aide du mécanisme du dossier de contrôle, vous pouvez générer des communications interactives pour le canal d’impression ou utiliser le service Output pour fusionner les données avec le modèle.

Cet article aborde le cas de la fusion des données avec un modèle à l’aide du service Output via le mécanisme du dossier de contrôle.

Le service Output est un service OSGi qui fait partie d’AEM Document Services. Le service Output prend en charge divers formats de sortie et fonctions de conception de sortie d’AEM Forms Designer. Le service Output peut convertir des modèles XFA et des données XML pour générer des documents d’impression dans différents formats.

Pour en savoir plus sur le service Output, [cliquez ici](https://helpx.adobe.com/fr/aem-forms/6/output-service.html).
Pour configurer le dossier de contrôle sur votre système, procédez comme suit :
* [Téléchargez et extrayez le contenu du fichier zip](assets/outputservicewatchedfolderkt.zip). Ce fichier zip contient le package pour la création du dossier de contrôle et des fichiers d’exemple afin de tester le service Output à l’aide du mécanisme du dossier de contrôle.
   * Système Windows

      * Importez outputservicewatchedfolder.zip dans AEM à l’aide du gestionnaire de packages.
      * Cela crée un dossier de contrôle appelé outputservicewatchedfolder sur votre lecteur C.
   * Système non Windows
      * [Ouvrir le paramètre de configuration du dossier de contrôle](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Définissez la propriété de chemin du dossier du nœud outservice pour qu’elle pointe vers un emplacement approprié.
      * Enregistrez vos modifications.
      * L’emplacement mentionné ci-dessus correspond à votre dossier de contrôle.

Déposez les dossiers SamplePdfFile et SampleXdpFile dans le dossier d’entrée du dossier de contrôle. Une fois le traitement des fichiers réussi, les résultats sont placés dans le dossier des résultats de votre dossier de contrôle.


>[!NOTE]
>
>Si le script associé au dossier de contrôle nécessite plusieurs fichiers, vous devez créer un dossier, y placer tous les fichiers requis et déposer le dossier dans le dossier d’entrée de votre dossier de contrôle.
>
>Si le script associé au dossier de contrôle ne nécessite qu’un seul fichier d’entrée, vous pouvez le déposer directement dans le dossier d’entrée de votre dossier de contrôle.
