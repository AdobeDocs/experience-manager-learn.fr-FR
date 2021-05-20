---
title: Chapitre 5 - Création de pages Content Services - Content Services
description: Le chapitre 5 du tutoriel AEM sans affichage couvre la création de pages à partir des modèles définis dans le chapitre 4. Ces pages se comportent comme les points de terminaison HTTP JSON.
feature: Fragments de contenu, API
topic: Sans affichage, gestion de contenu
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# Chapitre 5 - Création de pages Content Services

Le chapitre 5 du tutoriel AEM sans affichage couvre la création de la page à partir des modèles définis dans le chapitre 4. La page créée dans ce chapitre agit comme point de terminaison HTTP JSON pour l’application mobile.

>[!NOTE]
>
> L’architecture du contenu de la page de `/content/wknd-mobile/en/api` a été prédéfinie. Les pages de base de `en` et `api` ont un objectif architectural et organisationnel, mais ne sont pas strictement nécessaires. Si le contenu de l’API peut être localisé, il est recommandé de suivre les bonnes pratiques habituelles en matière de copie de la langue et d’organisation des pages de Multi-site Manager , car la page de l’API peut être localisée comme n’importe quelle page AEM Sites.

## Création de la page de l’API Event

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Appuyez sur le libellé de la page API**, puis sur le bouton  **** Créer dans la barre d’actions supérieure et créez une page API Events sous la page API.
   1. Appuyez sur **Créer** dans la barre d’actions supérieure.
   1. Sélectionnez le modèle **API d’événements**
   1. Dans le champ **Nom**, saisissez **events**.
   1. Dans le champ **Titre**, saisissez **API Events**.
   1. Appuyez sur **Créer** dans la barre d’actions supérieure pour créer la page.
   1. Appuyez sur **Terminé** pour revenir à l’administrateur AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Création de la page de l’API Events

>[!NOTE]
>
> Le projet fournit une page CSS afin de fournir des styles de base pour l’expérience de création.

1. Modifiez la page **API d’événements** en accédant à **AEM > Sites > WKND Mobile > Anglais > API**, en sélectionnant la page **API d’événements** et en appuyant sur **Modifier** dans la barre d’actions supérieure.
1. Ajoutez une **image de logo** à afficher dans l’application en la faisant glisser de l’outil de recherche de ressources vers l’espace réservé du composant Image.
   * Utilisez le logo fourni situé à l’adresse `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Ajoutez la **ligne de balise** à afficher au-dessus des événements.
   1. Modifier le composant **Texte**
   1. Saisissez le texte:
      * `The WKND is here.`

1. Sélectionnez les **événements** à afficher :
   1. Définissez la configuration suivante sur l’onglet **Propriétés** :
      * Modèle : **Événement**
      * Chemin d’accès parent : **/content/dam/wknd-mobile/en/events**
      * Balises : **&lt;Laissez vide>**
   1. Définissez la configuration suivante sur l’onglet **Éléments** :
      * Supprimez tous les éléments répertoriés pour vous assurer que TOUS les éléments des fragments de contenu d’événement sont exposés.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Examinez la sortie JSON de la page API.

La sortie JSON et son format peuvent être examinés en demandant la page avec le sélecteur `.model.json` .

Cette structure (ou schéma) JSON doit être bien comprise par les consommateurs de cette API. Il est essentiel que les clients de l’API comprennent quels aspects de la structure sont corrigés (c.-à-d. Logo (image) de l’API Event et Balisage actif (texte) fluides (par exemple, les événements répertoriés sous le composant Liste de fragments de contenu ).

La rupture de ce contrat sur une API publiée peut entraîner un comportement incorrect dans les applications consommatrices.

1. Dans les nouveaux onglets du navigateur, demandez les pages de l’API d’événements à l’aide du sélecteur `.model.json`, qui appelle l’exportateur JSON d’AEM Content Services, et sérialise la page et les composants dans une structure JSON normalisée et bien définie.

   La structure JSON générée par ces pages est la structure à laquelle les applications doivent s’aligner.

1. Demandez la page **API d’événements** en tant que **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Le résultat doit ressembler à ce qui suit :

![Sortie JSON AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Ce fichier JSON peut être généré de façon **ordonnée** (au format) pour une lisibilité humaine à l’aide du sélecteur `.tidy` :
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Étape suivante

Vous pouvez éventuellement installer le module de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur l’auteur AEM via [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 6 - Exposition du contenu sur la publication AEM au format JSON](./chapter-6.md)
