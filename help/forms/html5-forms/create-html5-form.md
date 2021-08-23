---
title: Création de Forms HTML5
description: Création et configuration de formulaires HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Développement
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '484'
ht-degree: 12%

---


# Création de formulaires HTML5

HTML5 forms est une nouvelle fonctionnalité d’Adobe Experience Manager qui offre le rendu des modèles de formulaire XFA (xdp) au format HTML5. Cette fonctionnalité permet le rendu des formulaires sur les périphériques mobiles et les navigateurs de bureau ne prenant pas en charge les documents XFA en PDF. Les formulaires HTML5 prennent en charge les fonctionnalités existantes des modèles de formulaires XFA, mais ajoutent également de nouvelles fonctionnalités, telles que la signature tactile, pour les appareils mobiles.

## Prérequis

Vérifiez que vous disposez d’une instance de travail d’AEM Forms. Veuillez suivre le [guide d’installation](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) pour installer et configurer AEM Forms.

## Créer votre premier formulaire HTML5

1. [Téléchargez et extrayez le contenu du fichier](assets/assets.zip) ZIP. Le fichier zip contient le fichier xdp et le fichier de données.
2. [Accès à Forms et aux documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Cliquez sur Créer -> Téléchargement du fichier
4. Sélectionnez le modèle xdp téléchargé à l’étape 2.

## Aperçu au format HTML

Le xdp peut être prévisualisé au format HTML5 ou PDF. Pour prévisualiser le fichier xdp au format HTML5, procédez comme suit :

* Appuyez sur le fichier xdp nouvellement chargé et cliquez sur _Aperçu -> Aperçu au format HTML_. Vous devriez voir le fichier xdp rendu au format HTML5.

>[!NOTE]
>Lorsque vous sélectionnez l’option _Aperçu au format PDF_ , le PDF rendu ne s’affiche pas dans le navigateur, car AEM Forms effectue le rendu des pdf dynamiques qui nécessitent le module externe Acrobat. Vous devez télécharger le PDF et l’ouvrir à l’aide d’Adobe Acrobat/Reader pour l’afficher.


## Aperçu avec des données

Pour prévisualiser le fichier xdp au format HTML5 avec le fichier de données, procédez comme suit :

* Appuyez sur le fichier xdp nouvellement chargé et cliquez sur _Aperçu -> Aperçu avec des données_. Recherchez et sélectionnez le fichier de données, puis cliquez sur _Aperçu_.
* Le modèle rendu au format HTML5 doit être prérenseigné avec les données.

## Explorer les propriétés avancées du modèle xdp

Les propriétés avancées du modèle xdp vous permettent de définir la date de publication, le gestionnaire d’envoi, le profil de rendu de votre formulaire, le service de préremplissage, etc. Pour afficher les propriétés avancées du modèle, appuyez sur xdp et cliquez sur _properties -> Advanced_. Vous y trouverez plusieurs propriétés. Certaines de ces propriétés sont décrites ici.

**URL d’envoi**  : il s’agit de l’URL qui gérera l’envoi du formulaire HTML5. Nous aborderons cela dans la prochaine leçon. Si aucune URL d’envoi n’est spécifiée ici, le gestionnaire d’envoi par défaut est appelé, ce qui renvoie les données de formulaire au navigateur.

**Profil de rendu HTML**  : les formulaires HTML5 ont la notion de profils qui sont exposés en tant que points de fin REST pour activer le rendu mobile des modèles de formulaire. La majorité des fois où le profil de rendu par défaut doit être suffisant pour générer le formulaire. Si le profil de rendu par défaut ne répond pas à vos besoins, un [profil personnalisé](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html) peut être créé et associé au formulaire.

**Service de préremplissage**  : le service de préremplissage est généralement utilisé pour remplir votre formulaire avec des données récupérées à partir d’une source de données d’arrière-plan.

