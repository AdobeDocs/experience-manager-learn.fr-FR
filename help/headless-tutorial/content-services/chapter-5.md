---
title: Chapitre 5 - Créer des pages Content Services - Content Services
description: Le chapitre 5 du tutoriel AEM Headless couvre la création de pages à partir des modèles définis dans le chapitre 4. Ces pages se comportent comme les point d’entrées du HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 227
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 100%

---

# Chapitre 5 - Créer des pages Content Services

Le chapitre 5 du tutoriel sur AEM Headless couvre la création de la page à partir des modèles définis dans le chapitre 4. La page créée dans ce chapitre agit comme point d’entrée du HTTP JSON pour l’application mobile.

>[!NOTE]
>
> L’architecture du contenu de la page de `/content/wknd-mobile/en/api` a été préconfigurée. Les pages de base de `en` et `api` ont un objectif architectural et organisationnel, mais ne sont pas strictement requises. Si le contenu de l’API peut être localisé, il est recommandé de suivre les bonnes pratiques habituelles en matière de copie de la langue et d’organisation des pages de Multi Site Manager, car la page de l’API peut être localisée comme n’importe quelle page AEM Sites.

## Créer la page de l’API d’événements

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Appuyez sur le libellé de la page d’API**, puis appuyez sur le bouton **Créer** dans la barre d’actions supérieure, et créez une page d’API d’événements sous la page d’API.
   1. Appuyez sur **Créer** dans la barre d’actions supérieure.
   1. Sélectionnez le modèle **API d’événements**.
   1. Dans le champ **Nom**, saisissez **événements**.
   1. Dans le champ **Titre**, saisissez **API d’événements**.
   1. Appuyez sur **Créer** dans la barre d’actions supérieure pour créer la page.
   1. Appuyez sur **Terminé** pour revenir à l’administration AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Créer la page de l’API d’événements

>[!NOTE]
>
> Le projet fournit une page CSS avec des styles de base pour l’expérience de création.

1. Modifiez la page d’**API d’événements** en accédant à **AEM > Sites > WKND Mobile > Anglais > API**, en sélectionnant la page d’**API d’événements** et en appuyant sur **Modifier** dans la barre d’actions supérieure.
1. Ajoutez une **image de logo** pour l’afficher dans l’application en la faisant glisser de l’outil de recherche de ressources vers l’espace réservé du composant Image.
   * Utilisez le logo fourni figurant sous `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Ajoutez une **ligne de balise** à afficher au-dessus des événements.
   1. Modifiez le composant de **Texte**.
   1. Saisissez le texte :
      * `The WKND is here.`

1. Sélectionnez les **événements** à afficher :
   1. Définissez la configuration suivante sur l’onglet **Propriétés** :
      * Modèle : **Événement**.
      * Chemin d’accès parent : **/content/dam/wknd-mobile/en/events**.
      * Balises : **&lt;Leave blank>**.
   1. Définissez la configuration suivante sur l’onglet **Éléments** :
      * Supprimez tous les éléments répertoriés pour vous assurer que TOUS les éléments des fragments de contenu d’événement sont exposés.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Examinez la sortie JSON de la page d’API.

La sortie JSON et son format peuvent être examinés en demandant la page avec le sélecteur `.model.json`.

Cette structure (ou schéma) JSON doit être bien comprise par les consommateurs et consommatrices de cette API. Il est essentiel que les clients et clientes de l’API comprennent quels aspects de la structure sont fixes (c’est-à-dire le logo (image) de l’API d’événements et la balise active (texte) et lesquels sont fluides, comme les événements répertoriés sous le composant Liste de fragments de contenu).

La rupture de ce contrat sur une API publiée peut entraîner un comportement incorrect dans les applications consommatrices.

1. Dans les nouveaux onglets du navigateur, demandez les pages d’API d’événements à l’aide du sélecteur `.model.json`, qui appelle l’exporteur JSON d’AEM Content Services et sérialise la page et les composants dans une structure JSON normalisée et bien définie.

   La structure JSON générée par ces pages est la structure sur laquelle les applications consommatrices doivent s’aligner.

1. Demandez la page d’**API d’événements** en tant que **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json).

   Le résultat doit ressembler à ce qui suit :

![Sortie JSON AEM Content Services.](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Ce fichier JSON peut être généré de façon **propre** (formaté) pour la lisibilité humaine à l’aide du sélecteur `.tidy` :
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json).

## Étape suivante

Si vous le souhaitez, vous pouvez installer le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur l’instance de création AEM via le [Gestionnaire de packages AEM](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 6 - Exposer le contenu sur l’instance de publication AEM au format JSON](./chapter-6.md)
