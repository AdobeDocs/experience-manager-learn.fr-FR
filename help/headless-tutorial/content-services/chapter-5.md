---
title: Chapitre 5 - Création de pages Content Services - Content Services
description: Le chapitre 5 du tutoriel AEM sans affichage couvre la création de pages à partir des modèles définis dans le chapitre 4. Ces pages se comportent comme les points de terminaison HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# Chapitre 5 - Création de pages Content Services

Le chapitre 5 du tutoriel AEM sans affichage couvre la création de la page à partir des modèles définis dans le chapitre 4. La page créée dans ce chapitre agit comme point de terminaison HTTP JSON pour l’application mobile.

>[!NOTE]
>
> L’architecture du contenu de la page de `/content/wknd-mobile/en/api` a été précompilé. Pages de base de `en` et `api` ont un objectif architectural et organisationnel, mais ne sont pas strictement requis. Si le contenu de l’API peut être localisé, il est recommandé de suivre les bonnes pratiques habituelles en matière de copie de la langue et d’organisation des pages de Multi-site Manager , car la page de l’API peut être localisée comme n’importe quelle page AEM Sites.

## Création de la page de l’API Event

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Appuyez sur le libellé de la page API.**, puis appuyez sur le bouton **Créer** dans la barre d’actions supérieure, puis créez une page API d’événements sous la page API.
   1. Appuyer **Créer** dans la barre d’actions supérieure
   1. Sélectionner **API des événements** modèle
   1. Dans le **Nom** entrée de champ **events**
   1. Dans le **Titre** entrée de champ **API des événements**
   1. Appuyer **Créer** dans la barre d’actions supérieure pour créer la page
   1. Appuyer **Terminé** pour revenir à l’administrateur AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Création de la page de l’API Events

>[!NOTE]
>
> Le projet fournit une page CSS afin de fournir des styles de base pour l’expérience de création.

1. Modifiez la variable **API des événements** en accédant à **AEM > Sites > WKND Mobile > Anglais > API**, en sélectionnant le **API des événements** page et appuyer **Modifier** dans la barre d’actions supérieure.
1. Ajouter un **image du logo** pour l’afficher dans l’application en la faisant glisser de l’outil de recherche de ressources vers l’espace réservé du composant Image.
   * Utilisez le logo fourni figurant dans `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Ajouter **ligne de balise** pour afficher au-dessus des événements.
   1. Modifiez la variable **Texte** component
   1. Saisissez le texte:
      * `The WKND is here.`

1. Sélectionnez la **events** pour afficher :
   1. Définissez la configuration suivante sur le **Propriétés** tab :
      * Modèle : **Événement**
      * Chemin d’accès parent : **/content/dam/wknd-mobile/en/events**
      * Balises : **&lt;leave blank=&quot;&quot;>**
   1. Définissez la configuration suivante sur le **Éléments** tab :
      * Supprimez tous les éléments répertoriés pour vous assurer que TOUS les éléments des fragments de contenu d’événement sont exposés.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Examinez la sortie JSON de la page API.

La sortie JSON et son format peuvent être examinés en demandant la page avec l’événement `.model.json` sélecteur.

Cette structure (ou schéma) JSON doit être bien comprise par les consommateurs de cette API. Il est essentiel que les clients de l’API comprennent quels aspects de la structure sont corrigés (c.-à-d. Logo (image) de l’API Event et Balisage actif (texte) fluides (par exemple, les événements répertoriés sous le composant Liste de fragments de contenu ).

La rupture de ce contrat sur une API publiée peut entraîner un comportement incorrect dans les applications consommatrices.

1. Dans les nouveaux onglets du navigateur, demandez les pages de l’API Events à l’aide de la fonction `.model.json` le sélecteur, qui appelle l’exportateur JSON de Content Services AEM et sérialise la page et les composants dans une structure JSON normalisée et bien définie.

   La structure JSON générée par ces pages est la structure à laquelle les applications doivent s’aligner.

1. Demandez au **API des événements** page en tant que **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Le résultat doit ressembler à ce qui suit :

![Sortie JSON AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Ce fichier JSON peut être généré dans une **tidy** Mode (formaté) pour la lisibilité humaine à l’aide de la fonction `.tidy` sélecteur :
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Étape suivante

Si vous le souhaitez, vous pouvez installer la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) module de contenu sur l’auteur AEM via [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 6 - Exposition du contenu sur la publication AEM au format JSON](./chapter-6.md)
