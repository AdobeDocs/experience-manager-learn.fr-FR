---
title: Création de Forms HTML5
description: Création et configuration de formulaires HTML5
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 11%

---


# Création de formulaires HTML5

HTML5 forms est une nouvelle fonctionnalité de Adobe Experience Manager qui offre le rendu des modèles de formulaires XFA (xdp) au format HTML5. Cette fonctionnalité permet le rendu des formulaires sur les périphériques mobiles et les navigateurs de bureau ne prenant pas en charge les documents XFA en PDF. Les formulaires HTML5 prennent en charge les fonctionnalités existantes des modèles de formulaires XFA, mais ajoutent également de nouvelles fonctionnalités, telles que la signature tactile, pour les appareils mobiles.

## Condition requise

Assurez-vous d&#39;avoir une instance de travail d&#39;AEM Forms. Suivez le [guide d&#39;installation](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) pour installer et configurer AEM Forms.

## Créer votre premier formulaire HTML5

1. [Téléchargez et extrayez le contenu du fichier](assets/assets.zip) zip. Le fichier zip contient xdp et le fichier de données
2. [Accédez à Forms et aux Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Cliquez sur Créer -> Télécharger le fichier
4. Sélectionnez le modèle xdp téléchargé à l’étape 2.

## Aperçu au format HTML

Le fichier xdp peut être prévisualisé au format HTML5 ou PDF. Pour prévisualisation du fichier xdp au format HTML5, suivez les étapes ci-après.

* Appuyez sur le fichier xdp nouvellement chargé et cliquez sur _Prévisualisation -> Prévisualisation au format HTML_. Vous devriez voir le fichier xdp rendu au format HTML5

>[!NOTE]
>Lorsque vous sélectionnez l’option _Prévisualisation en tant que PDF_, le fichier PDF rendu ne s’affiche pas dans le navigateur car AEM Forms génère des fichiers PDF dynamiques qui nécessitent un module externe Acrobat. Vous devez télécharger le fichier PDF et l’ouvrir à l’aide de Adobe Acrobat/Reader vers vue.


## Aperçu avec des données

Pour prévisualisation du fichier xdp au format HTML5 avec le fichier de données, procédez comme suit :

* Appuyez sur le fichier xdp nouvellement chargé et cliquez sur _Prévisualisation -> Prévisualisation avec Data_. Recherchez et sélectionnez le fichier de données, puis cliquez sur _Prévisualisation_.
* Vous devriez voir le modèle rendu au format HTML5 prérempli avec les données.

## Explorer les propriétés avancées du modèle xdp

Les propriétés avancées du modèle xdp vous permettent de spécifier la date de publication, le gestionnaire d’envoi, le profil de rendu de votre formulaire, le service de préremplissage, etc. Pour vue aux propriétés avancées du modèle, appuyez sur le fichier xdp et cliquez sur _properties -> Advanced_. Vous trouverez ici un certain nombre de propriétés. Certaines de ces propriétés sont couvertes ici.

**URL**  d’envoi : URL qui gère l’envoi du formulaire HTML5. Nous en parlerons dans la prochaine leçon. Si aucune URL d’envoi n’est spécifiée ici, le gestionnaire d’envoi par défaut est appelé, ce qui renvoie les données du formulaire au navigateur.

**PROFIL**  de rendu HTML - Les formulaires HTML5 ont la notion de Profils qui sont exposés en tant que points de terminaison REST pour activer le rendu mobile des modèles de formulaire. La majorité des fois où le profil de rendu par défaut doit être suffisant pour générer le formulaire. Si le profil de rendu par défaut ne répond pas à vos besoins, un [profil personnalisé](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) peut être créé et associé au formulaire.

**Service**  de préremplissage : le service de préremplissage est généralement utilisé pour remplir votre formulaire avec des données extraites d’une source de données principale.

