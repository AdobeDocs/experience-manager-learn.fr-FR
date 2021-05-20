---
title: Utilisation D’Un Modèle De Données De Formulaire Pour Publier Des Données Binaires
seo-title: Utilisation D’Un Modèle De Données De Formulaire Pour Publier Des Données Binaires
description: Publication de données binaires dans AEM DAM à l’aide du modèle de données de formulaire
seo-description: Publication de données binaires dans AEM DAM à l’aide du modèle de données de formulaire
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---


# Utilisation D’Un Modèle De Données De Formulaire Pour Publier Des Données Binaires{#using-form-data-model-to-post-binary-data}

À compter d’AEM Forms 6.4, nous pouvons désormais appeler le service de modèle de données de formulaire comme étape dans AEM Workflow. Cet article vous guide tout au long d’un exemple de cas d’utilisation pour la publication d’un document d’enregistrement à l’aide du service de modèle de données de formulaire.

Le cas pratique est le suivant :

1. Un utilisateur remplit et envoie le formulaire adaptatif.
1. Le formulaire adaptatif est configuré pour générer un document d’enregistrement.
1. Lors de l’envoi de ces formulaires adaptatifs, AEM processus est déclenché, qui utilise le service de modèle de données de formulaire pour POST le document d’enregistrement à AEM DAM.

![posttodam](assets/posttodamshot1.png)

Onglet Modèle de données de formulaire - Propriétés

Dans l’onglet Service Input , nous associons les éléments suivants :

* file (Objet binaire devant être stocké) avec la propriété DOR.pdf relative à la charge utile. Cela signifie que lorsque le formulaire adaptatif est envoyé, le document d’enregistrement généré est stocké dans un fichier appelé DOR.pdf par rapport à la charge utile du processus.**Assurez-vous que ce fichier DOR.pdf est identique à celui fourni lors de la configuration de la propriété d’envoi du formulaire adaptatif.**

* fileName : il s’agit du nom par lequel l’objet binaire sera stocké dans DAM. Vous souhaitez donc que cette propriété soit générée dynamiquement, de sorte que chaque fileName soit unique par envoi. À cette fin, nous avons utilisé l’étape de processus dans le processus pour créer une propriété de métadonnées appelée filename et définir sa valeur sur la combinaison du nom du membre et du numéro de compte de la personne qui envoie le formulaire. Par exemple, si le nom du membre de la personne est John Jacobs et que son numéro de compte est le 9846, le nom de fichier sera John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Service Input

>[!NOTE]
>
>Conseils de dépannage : si, pour une raison quelconque, le fichier DOR.pdf n’est pas créé dans la gestion des ressources numériques, réinitialisez les paramètres d’authentification de la source de données en cliquant sur [ici](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Il s’agit des paramètres d’authentification AEM, qui sont par défaut admin/admin.

Pour tester cette fonctionnalité sur votre serveur, procédez comme suit :

1.[Déployez le lot Developer withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Téléchargez et déployez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Ce lot OSGI personnalisé est utilisé pour créer une propriété de métadonnées et définir sa valeur à partir des données de formulaire envoyées.

1. [Importez les ](assets/postdortodam.zip) ressources associées à cet article dans AEM à l’aide du gestionnaire de packages. Vous obtiendrez ce qui suit :

   1. Modèle de processus
   1. Formulaire adaptatif configuré pour être envoyé au processus AEM
   1. Source de données configurée pour utiliser le fichier PostToDam.JSON
   1. Modèle de données de formulaire qui utilise la source de données

1. Pointez votre navigateur [pour ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
1. Remplissez le formulaire et envoyez-le.
1. Vérifiez l’application Assets si le document d’enregistrement est créé et stocké.


[Swagger ](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) Filtré lors de la création de la source de données est disponible à titre de référence.
