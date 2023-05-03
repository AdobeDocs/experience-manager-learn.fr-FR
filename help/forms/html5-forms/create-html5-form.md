---
title: Création de Forms HTML5
description: Création et configuration de HTML5 forms
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 6%

---

# Création de formulaires HTML5

HTML5 forms est une nouvelle fonctionnalité d’Adobe Experience Manager qui offre le rendu des modèles de formulaire XFA (xdp) au format HTML5. Cette fonctionnalité permet le rendu des formulaires sur les périphériques mobiles et les navigateurs de bureau ne prenant pas en charge les documents XFA en PDF. Les formulaires HTML5 prennent non seulement en charge les fonctionnalités existantes des modèles de formulaires XFA, mais ajoutent également de nouvelles fonctionnalités, telles que la signature tactile, pour les appareils mobiles.

## Prérequis

Vérifiez que vous disposez d’une instance de travail d’AEM Forms. Veuillez suivre le [guide d’installation](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) pour installer et configurer AEM Forms

## Créer votre premier HTML5

1. [Télécharger et extraire le contenu du fichier zip](assets/assets.zip). Le fichier zip contient le fichier xdp et le fichier de données.
2. [Accès à Forms et aux documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Cliquez sur Créer -> Téléchargement du fichier
4. Sélectionnez le modèle xdp téléchargé à l’étape 2.

## Aperçu en tant que HTML

Le xdp peut être prévisualisé au format HTML5 ou PDF. Pour prévisualiser le fichier xdp au format HTML5, procédez comme suit :

* Appuyez sur le fichier xdp nouvellement chargé, puis cliquez sur _Aperçu -> Aperçu en tant que HTML_. Vous devriez voir le fichier xdp rendu en tant que HTML5.

>[!NOTE]
>Lorsque vous sélectionnez _Aperçu en tant que PDF_ l’option du PDF rendu ne s’affiche pas dans le navigateur, car AEM Forms effectue le rendu des pdf dynamiques qui nécessitent le module externe Acrobat. Vous devez télécharger le PDF et l’ouvrir à l’aide d’Adobe Acrobat/Reader pour l’afficher.


## Aperçu avec des données

Pour prévisualiser le fichier xdp au format HTML5 avec le fichier de données, procédez comme suit :

* Appuyez sur le fichier xdp nouvellement chargé, puis cliquez sur _Aperçu -> Aperçu avec des données_. Recherchez et sélectionnez le fichier de données, puis cliquez sur _Aperçu_.
* Vous devriez voir le modèle rendu au format HTML5 prérempli avec les données.

## Explorer les propriétés avancées du modèle xdp

Les propriétés avancées du modèle xdp vous permettent de définir la date de publication, le gestionnaire d’envoi, le profil de rendu de votre formulaire, le service de préremplissage, etc. Pour afficher les propriétés avancées du modèle, appuyez sur le fichier xdp et cliquez sur _properties -> Advanced_. Vous y trouverez plusieurs propriétés. Certaines de ces propriétés sont décrites ici.

**Submit URL** - Il s’agit de l’URL qui gérera l’envoi du formulaire HTML5. Nous aborderons cela dans la prochaine leçon. Si aucune URL d’envoi n’est spécifiée ici, le gestionnaire d’envoi par défaut est appelé, ce qui renvoie les données de formulaire au navigateur.

**Profil de rendu de HTML** - Les formulaires HTML5 ont la notion de Profils qui sont exposés en tant que points de fin REST pour activer le rendu mobile des modèles de formulaire. La majorité des fois où le profil de rendu par défaut doit être suffisant pour générer le formulaire. Si le profil de rendu par défaut ne répond pas à vos besoins, une [profil personnalisé](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) peut être créé et associé au formulaire.

**Service de préremplissage** - Le service de préremplissage est généralement utilisé pour remplir votre formulaire avec des données récupérées à partir d’une source de données d’arrière-plan.
