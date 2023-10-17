---
title: Créer des formulaires HTML5
description: Créer et configurer des formulaires HTML5
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
workflow-type: ht
source-wordcount: '481'
ht-degree: 100%

---

# Créer des formulaires HTML5

Les formulaires HTML5 constituent une nouvelle fonctionnalité dans Adobe Experience Manager qui offre le rendu des modèles de formulaires XFA (XDP) au format HTML5. Cette fonctionnalité permet le rendu des formulaires sur les appareils mobiles et les navigateurs de bureau ne prenant pas en charge les documents XFA en PDF. Les formulaires HTML5 prennent en charge les fonctionnalités existantes des modèles de formulaires XFA, mais ajoutent également de nouvelles fonctionnalités, telles que la signature tactile, pour les appareils mobiles.

## Prérequis

Vérifiez que vous disposez d’une instance fonctionnelle d’AEM Forms. Veuillez suivre le [guide d’installation](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=fr) pour installer et configurer AEM Forms.

## Créer votre premier formulaire HTML5

1. [Téléchargez et procédez à l’extraction du contenu du fichier ZIP](assets/assets.zip). Le fichier ZIP contient le fichier XDP et le fichier de données.
2. [Accédez à Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
3. Cliquez sur Créer > Charger un fichier.
4. Sélectionnez le modèle xdp téléchargé à l’étape 2.

## Prévisualiser au format HTML

Le fichier XDP peut être prévisualisé au format HTML5 ou PDF. Pour prévisualiser le fichier XDP au format HTML5, procédez comme suit :

* Appuyez sur le fichier XDP nouvellement chargé, puis cliquez sur _Prévisualiser -> Prévisualiser en tant que HTML_. Vous devriez voir le fichier XDP rendu en tant que HTML5.

>[!NOTE]
>Lorsque vous sélectionnez l’option _Prévisualiser en tant que PDF_, le PDF rendu ne s’affiche pas dans le navigateur, car AEM Forms effectue le rendu des PDF dynamiques qui nécessitent le plug-in Acrobat. Vous devez télécharger le PDF et l’ouvrir à l’aide d’Adobe Acrobat/Reader pour l’afficher.


## Aperçu avec des données

Pour prévisualiser le fichier XDP au format HTML5 avec le fichier de données, procédez comme suit :

* Appuyez sur le fichier XDP nouvellement chargé, puis cliquez sur _Prévisualiser -> Prévisualiser avec des données_. Recherchez et sélectionnez le fichier de données, puis cliquez sur _Prévisualiser_.
* Vous devriez voir le modèle rendu au format HTML5 prérempli avec les données.

## Explorer les propriétés avancées du modèle XDP

Les propriétés avancées du modèle XDP vous permettent de définir la date de publication, le gestionnaire d’envoi, le profil de rendu de votre formulaire, le service de préremplissage, etc. Pour afficher les propriétés avancées du modèle, appuyez sur le fichier XDP et cliquez sur _Propriétés -> Avancé_. Vous y trouverez plusieurs propriétés. Certaines de ces propriétés sont décrites ici.

**URL d’envoi** : il s’agit de l’URL qui gère l’envoi du formulaire HTML5. Nous aborderons cela dans la prochaine leçon. Si aucune URL d’envoi n’est spécifiée ici, le gestionnaire d’envoi par défaut est appelé, ce qui renvoie les données de formulaire au navigateur.

**Profil de rendu HTML** : les formulaires HTML5 intègrent la notion de Profils, lesquels sont exposés en tant que points d’entrée REST pour activer le rendu sur périphériques mobile des modèles de formulaire. Dans la plupart des cas, le profil de rendu par défaut devrait suffire à effectuer le rendu du formulaire. Si le profil de rendu par défaut ne répond pas à vos besoins, un [profil personnalisé](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html?lang=fr) peut être créé et associé au formulaire.

**Service de préremplissage** : le service de préremplissage est généralement utilisé pour remplir votre formulaire avec des données récupérées à partir d’une source de données back-end.
