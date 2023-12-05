---
title: Chapitre 2 - Définition de modèles de fragments de contenu d’événement - Content Services
description: Le chapitre 2 du tutoriel AEM Headless porte sur l’activation et la définition des modèles de fragment de contenu utilisés pour définir une structure de données normalisée et une interface de création pour la création d’événements.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 472
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 100%

---

# Chapitre 2 - Utilisation de modèles de fragment de contenu

Les modèles de fragment de contenu AEM définissent des schémas de contenu qui peuvent être utilisés pour modéliser la création de contenu brut par les créateurs et créatrices AEM. Cette approche est similaire à la génération de modèles automatique ou à la création basée sur des formulaires. Le concept clé avec les fragments de contenu est que le contenu créé est indépendant de la présentation, c’est-à-dire qu’il est destiné à une utilisation multicanal où l’application consommatrice, qu’il s’agisse d’AEM, d’une application d’une seule page ou d’une application mobile, contrôle la manière dont le contenu s’affiche pour l’utilisateur et l’utilisatrice.

La principale préoccupation du fragment de contenu est de garantir que :

1. Le contenu correct est collecté auprès du créateur ou de la créatrice.
2. Le contenu peut être exposé dans un format structuré et bien compris des applications consommatrices.

Ce chapitre traite de l’activation et de la définition des modèles de fragments de contenu utilisés pour définir une structure de données normalisée et une interface de création pour la modélisation et la création d’« événements ».

## Activer les modèles de fragment de contenu

Les modèles de fragment de contenu **doivent** être activés via **[le [!UICONTROL navigateur de configuration]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=fr) d’AEM**.

Si les modèles de fragment de contenu ne sont **pas** activés pour une configuration, le bouton **[!UICONTROL Créer] > [!UICONTROL Fragment de contenu]** ne s’affichera pas pour la configuration AEM appropriée.

>[!NOTE]
>
>Les configurations AEM représentent un ensemble de [configurations clientes basées sur le contexte](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) stocké sous `/conf`. En règle générale, les configurations AEM sont corrélées à un site web particulier géré dans AEM Sites ou à une unité opérationnelle responsable d’un sous-ensemble de contenu (ressources, pages, etc.) dans AEM.
>
>Pour qu’une configuration affecte une hiérarchie de contenu, elle doit être référencée via la propriété `cq:conf` sur cette hiérarchie de contenu. (Cela est réalisé pour la configuration [!DNL WKND Mobile] à l’**étape 5** ci-dessous).
>
>Lorsque la configuration `global` est utilisée, elle s’applique à tout le contenu, et `cq:conf` ne doit pas être défini.
>
>Pour plus d’informations, consultez la [[!UICONTROL documentation relative au ]Navigateur de configurations](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=fr).

1. Connectez-vous au service de création d’AEM en tant qu’utilisateur ou utilisatrice avec les autorisations appropriées pour modifier la configuration concernée.
   * Pour ce tutoriel, l’utilisateur ou utilisatrice **admin** peut être utilisée.
1. Accédez à **[!UICONTROL Outil] > [!UICONTROL Général] > [!UICONTROL Navigateur de configuration]**.
1. Appuyez sur l’**icône de dossier** en regard de **[!DNL WKND Mobile]** pour sélectionner, puis appuyez sur le bouton **[!UICONTROL Modifier]** en haut à gauche.
1. Sélectionnez **[!UICONTROL Modèles de fragment de contenu]**, puis appuyez sur **[!UICONTROL Enregistrer et fermer]** en haut à droite.

   Cela active les modèles de fragment de contenu sur les arborescences de contenu du dossier de ressources où la propriété [!DNL WKND Mobile] est appliquée.

   >[!NOTE]
   >
   >Cette modification de configuration n’est pas réversible à partir de l’IU web [!UICONTROL Configuration AEM]. Pour annuler cette configuration :
   >    
   >    1. Ouvrez [CRXDE Lite](http://localhost:4502/crx/de).
   >    1. Accédez à `/conf/wknd-mobile/settings/dam/cfm`.
   >    1. Supprimez le nœud `models`.
   >    
   >Tous les modèles de fragment de contenu existants créés sous cette configuration sont supprimés, et leurs définitions sont stockées sous `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Appliquez la configuration **[!DNL WKND Mobile]** au **[!DNL WKND Mobile]dossier de ressources** pour permettre la création de fragments de contenu à partir de modèles de fragment de contenu dans cette hiérarchie de dossier de ressources :

   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Ressources] > [!UICONTROL Fichiers]**.
   1. Sélectionnez le **[!UICONTROL dossier WKND mobile]**.
   1. Appuyez sur le bouton **[!UICONTROL Propriétés]** dans la barre d’actions supérieure pour ouvrir [!UICONTROL Propriétés du dossier].
   1. Dans [!UICONTROL Propriétés du dossier], appuyez sur l’onglet **[!UICONTROL Services cloud]**.
   1. Vérifiez que le champ **[!UICONTROL Configuration du cloud]** est défini sur **/conf/wknd-mobile**.
   1. Appuyez sur **[!UICONTROL Enregistrer et fermer]** dans le coin supérieur droit pour conserver les modifications.

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> Les __modèles de fragment de contenu__ ont été déplacés de __Outils > Ressources__ vers __Outils > Général__.

## Comprendre le modèle de fragment de contenu à créer

Avant de définir notre modèle de fragment de contenu, examinons l’expérience que nous allons créer pour nous assurer que nous capturons tous les points de données nécessaires. Pour ce faire, nous allons vérifier la conception des applications mobiles et mapper les éléments de conception au contenu à collecter.

Nous pouvons répertorier les points de données qui définissent un événement comme suit :

![Création d’un modèle de fragment de contenu](assets/chapter-2/design-to-model-mapping.png)

Avec le mappage, nous pouvons définir le fragment de contenu utilisé pour collecter et exposer à terme les données d’événement.

## Création d’un modèle de fragment de contenu

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles de fragment de contenu]**.
1. Appuyez sur le dossier **[!DNL WKND Mobile]** pour l’ouvrir.
1. Appuyez sur **[!UICONTROL Créer]** pour ouvrir l’assistant de création d’un modèle de fragment de contenu.
1. Saisissez **[!DNL Event]** comme **[!UICONTROL Titre du modèle]** *(description facultative)* et appuyez sur **[!UICONTROL Créer]** pour enregistrer.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Définition de la structure du modèle de fragment de contenu

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles de fragment de contenu] >[!DNL WKND]**.
1. Sélectionnez le **[!DNL Event]** modèle de fragment de contenu et cliquez sur **[!UICONTROL Modifier]** dans la barre d’action du haut.
1. À partir de l’**[!UICONTROL onglet de droite Types de données]**, faites glisser la **[!UICONTROL Saisie de texte sur une seule ligne]** dans la zone de dépôt de gauche pour définir le champ **[!DNL Question]**.
1. Veillez à ce que la nouvelle **[!UICONTROL Saisie de texte sur une seule ligne]** soit sélectionnée à gauche, et que l’onglet **[!UICONTROL Propriétés]** soit sélectionné à droite. Renseignez les champs Propriétés comme suit :

   * [!UICONTROL Rendre en tant que] : `textfield`.
   * [!UICONTROL Libellé du champ] : `Event Title`.
   * [!UICONTROL Nom de la propriété] : `eventTitle`.
   * [!UICONTROL Longueur maximale] : 25
   * [!UICONTROL Obligatoire] : `Yes`.

Répétez ces étapes en utilisant les définitions d’entrée définies ci-dessous pour créer le reste du modèle de fragment de contenu d’événement.

>[!NOTE]
>
> Le champ **Nom de la propriété** DOIT correspondre exactement, car l’application Android est programmée pour saisir ces noms.

### Description d’événement

* [!UICONTROL Type de données] : `Multi-line text`.
* [!UICONTROL Libellé du champ] : `Event Description`.
* [!UICONTROL Nom de la propriété] : `eventDescription`.
* [!UICONTROL Type par défaut] : `Rich text`.

### Date et heure de l’évènement

* [!UICONTROL Type de données] : `Date and time`.
* [!UICONTROL Libellé du champ] : `Event Date and Time`.
* [!UICONTROL Nom de la propriété] : `eventDateAndTime`.
* [!UICONTROL Obligatoire] : `Yes`.

### Type d’événement

* [!UICONTROL Type de données] : `Enumeration`.
* [!UICONTROL Libellé du champ] : `Event Type`.
* [!UICONTROL Nom de la propriété] : `eventType`.
* [!UICONTROL Options] : `Art,Music,Performance,Photography`.

### Prix du billet

* [!UICONTROL Type de données] : `Number`.
* [!UICONTROL Rendre en tant que] : `numberfield`.
* [!UICONTROL Libellé du champ] : `Ticket Price`.
* [!UICONTROL Nom de la propriété] : `eventPrice`.
* [!UICONTROL Type] : `Integer`.
* [!UICONTROL Obligatoire] : `Yes`.

### Image d’événement

* [!UICONTROL Type de données] : `Content Reference`.
* [!UICONTROL Rendre en tant que] : `contentreference`.
* [!UICONTROL Libellé du champ] : `Event Image`.
* [!UICONTROL Nom de la propriété] : `eventImage`.
* [!UICONTROL Chemin d’accès racine] : `/content/dam/wknd-mobile/images`.
* [!UICONTROL Obligatoire] : `Yes`.

### Nom du lieu

* [!UICONTROL Type de données] : `Single-line text`.
* [!UICONTROL Rendre en tant que] : `textfield`.
* [!UICONTROL Libellé du champ] : `Venue Name`.
* [!UICONTROL Nom de la propriété] : `venueName`.
* [!UICONTROL Longueur maximale] : 20
* [!UICONTROL Obligatoire] : `Yes`.

### Ville du lieu

* [!UICONTROL Type de données] : `Enumeration`.
* [!UICONTROL Libellé du champ] : `Venue City`.
* [!UICONTROL Nom de la propriété] : `venueCity`.
* [!UICONTROL Options] : `Basel,London,Los Angeles,Paris,New York,Tokyo`.

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>Le **[!UICONTROL Nom de la propriété]** désigne **à la fois** le nom de la propriété JCR où cette valeur est stockée ainsi que la clé dans le fichier JSON. Il doit s’agir d’un nom sémantique qui ne changera pas au cours de la durée de vie du modèle de fragment de contenu.

Après avoir créé le modèle de fragment de contenu, vous devez obtenir une définition qui ressemble à celle-ci :


![Modèle de fragment de contenu d’événement](assets/chapter-2/event-content-fragment-model.png)

## Étape suivante

Si vous le souhaitez, vous pouvez installer le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur l’instance de création AEM via [le gestionnaire de modules d’AEM](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans cette partie du tutoriel.

* [Chapitre 3 - Création de fragments de contenu d’événement](./chapter-3.md)
