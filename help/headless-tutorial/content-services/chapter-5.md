---
title: Chapitre 5 - Création de pages Content Services - Content Services
description: Le chapitre 5 du didacticiel AEM sans en-tête couvre la création des pages à partir des modèles définis au chapitre 4. Ces pages serviront de points de terminaison HTTP JSON.
feature: Fragments de contenu, API
topic: Sans tête, Gestion de contenu
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 1%

---


# Chapitre 5 - Création de pages Content Services

Le chapitre 5 du didacticiel AEM sans en-tête couvre la création de la page à partir des modèles définis au chapitre 4. La page créée dans ce chapitre servira de point de terminaison HTTP JSON pour l’application mobile.

>[!NOTE]
>
> L&#39;architecture de contenu de page de `/content/wknd-mobile/en/api` a été prédéfinie. Les pages de base de `en` et `api` ont un but architectural et organisationnel, mais ne sont pas strictement nécessaires. Si le contenu de l’API peut être localisé, il est recommandé de suivre les bonnes pratiques habituelles en matière de copie de langue et d’organisation des pages du gestionnaire multisite, car la page de l’API peut être localisée comme n’importe quelle page AEM Sites.

## Création de la page API Événement

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Appuyez sur le libellé de la page** API, puis sur le bouton  **** Créer dans la barre d’actions supérieure et créez une page API Événements sous la page API.
   1. Appuyez sur **Créer** dans la barre d’actions supérieure.
   1. Sélectionner le modèle **API Événements**
   1. Dans le champ **Nom**, saisissez **événements**.
   1. Dans le champ **Titre**, saisissez **API de Événements**.
   1. Appuyez sur **Créer** dans la barre d’actions supérieure pour créer la page.
   1. Appuyez sur **Terminé** pour revenir à l’administrateur AEM Sites.

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Création de la page API Événements

>[!NOTE]
>
> Le projet fournit une page CSS afin de fournir quelques styles de base pour l’expérience d’auteur.

1. Modifiez la page **API Événements** en accédant à **AEM > Sites > WKND Mobile > Anglais > API**, en sélectionnant la page **API Événements** et en appuyant sur **Modifier** dans la barre d’actions supérieure.
1. Ajoutez une **image de logo** à afficher dans l’application en la faisant glisser depuis l’outil de recherche de ressources vers l’espace réservé du composant Image.
   * Utilisez le logo fourni situé à `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Ajoutez **ligne de balise** pour qu’elle s’affiche au-dessus des événements.
   1. Modifier le composant **Texte**
   1. Saisissez le texte:
      * `The WKND is here.`

1. Sélectionnez les **événements** à afficher :
   1. Définissez la configuration suivante dans l&#39;onglet **Propriétés** :
      * Modèle : **Événement**
      * Chemin d’accès parent : **/content/dam/wknd-mobile/fr/événements**
      * Balises : **&lt;Ne rien indiquer>**
   1. Définissez la configuration suivante sur l’onglet **Eléments** :
      * Supprimez tous les éléments répertoriés afin de vous assurer que TOUS les éléments des fragments de contenu du Événement sont exposés.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Examiner la sortie JSON de la page API

La sortie JSON et son format peuvent être révisés en demandant la page avec le sélecteur `.model.json`.

Cette structure (ou ce schéma) JSON doit être bien comprise par les utilisateurs de cette API. Il est essentiel que les consommateurs d&#39;API comprennent quels aspects de la structure sont fixes (c.-à-d. Le logo (image) et la balise active (texte) de l’API Événement et qui sont fluides (par ex. les événements répertoriés sous le composant Liste de fragments de contenu).

La rupture de ce contrat sur une API publiée peut entraîner un comportement incorrect dans les applications consommatrices.

1. Dans les nouveaux onglets du navigateur, demandez les pages de l’API Événements à l’aide du sélecteur `.model.json`, qui appelle l’Exportateur JSON d’AEM Content Services, et sérialise la page et les composants dans une structure JSON normalisée et bien définie.

   La structure JSON générée par ces pages correspond à la structure à laquelle les applications doivent s’aligner.

1. Demandez à la page **Événements API** la valeur **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   Le résultat doit ressembler à :

![Sortie JSON de Content Services AEM](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Ce fichier JSON peut être généré de façon **ordonnée** (formatée) pour la lisibilité humaine à l’aide du sélecteur `.tidy` :
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Étape suivante

Vous pouvez éventuellement installer le [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) package de contenu sur AEM Author via [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce didacticiel et les chapitres précédents.

* [Chapitre 6 - Exposition de contenu sur AEM Publish en tant que JSON](./chapter-6.md)
