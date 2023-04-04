---
title: Chapitre 2 - Définition des modèles de fragment de contenu d’événement - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: Le chapitre 2 du tutoriel AEM sans affichage porte sur l’activation et la définition des modèles de fragment de contenu utilisés pour définir une structure de données normalisée et une interface de création pour la création d’événements.
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 12%

---

# Chapitre 2 - Utilisation de modèles de fragment de contenu

AEM les modèles de fragment de contenu définissent des schémas de contenu qui peuvent être utilisés pour modéliser la création de contenu brut par les auteurs AEM. Cette approche est similaire à la génération de modèles automatique ou à la création basée sur des formulaires. Le concept clé avec les fragments de contenu est que le contenu créé est indépendant de la présentation, c’est-à-dire qu’il est destiné à une utilisation multicanal où l’application consommatrice, qu’il s’agisse d’AEM, d’une application monopage ou d’une application mobile, contrôle la manière dont le contenu s’affiche pour l’utilisateur.

La Principale préoccupation du fragment de contenu est de garantir :

1. Le contenu correct est collecté auprès de l’auteur.
2. Le contenu peut être exposé dans un format structuré et bien compris aux applications gourmandes.

Ce chapitre traite de l’activation et de la définition des modèles de fragments de contenu utilisés pour définir une structure de données normalisée et une interface de création pour la modélisation et la création d’&quot;événements&quot;.

## Activation des modèles de fragment de contenu  

Modèles de fragment de contenu **must** être activé via **[AEM [!UICONTROL Explorateur de configuration]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=fr)**.

Si les modèles de fragment de contenu sont **not** activée pour une configuration, la variable **[!UICONTROL Créer] > [!UICONTROL Fragment de contenu]** ne s’affiche pas pour la configuration d’AEM appropriée.

>[!NOTE]
>
>Les configurations AEM représentent un ensemble de [configurations de client basées sur le contexte](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) stocké sous `/conf`. En règle générale, les configurations AEM sont corrélées à un site Web particulier géré dans AEM Sites ou à une unité opérationnelle responsable d’un sous-ensemble de contenu (ressources, pages, etc.) dans AEM.
>
>Pour qu&#39;une configuration affecte une hiérarchie de contenu, elle doit être référencée via la fonction `cq:conf` sur cette hiérarchie de contenu. (Cela est réalisé pour la variable [!DNL WKND Mobile] configuration dans **Étape 5** ci-dessous).
>
>Lorsque la variable `global` est utilisée, la configuration s’applique à tout le contenu, et `cq:conf` ne doit pas être défini.
>
>Pour plus d’informations, consultez la documentation relative au [[!UICONTROL Navigateur de configuration].](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=fr)

1. Connectez-vous à l’auteur AEM en tant qu’utilisateur avec les autorisations appropriées pour modifier la configuration appropriée.
   * Pour ce tutoriel, la variable **admin** peut être utilisé.
1. Accédez à **[!UICONTROL Outil] > [!UICONTROL Général] > [!UICONTROL Explorateur de configuration]**
1. Appuyez sur le bouton **icône de dossier** en regard de **[!DNL WKND Mobile]** pour sélectionner, puis appuyez sur la propriété **[!UICONTROL Modifier] button** en haut à gauche.
1. Sélectionner **[!UICONTROL Modèles de fragment de contenu]**, puis appuyez sur **[!UICONTROL Enregistrer et fermer]** en haut à droite.

   Cela active les modèles de fragment de contenu sur les arborescences de contenu du dossier de ressources qui ont la propriété [!DNL WKND Mobile] configuration appliquée.

   >[!NOTE]
   >
   >Cette modification de configuration n’est pas réversible à partir de la [!UICONTROL Configuration AEM] Interface utilisateur web. Pour annuler cette configuration :
   >    
   >    1. Ouvrez [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. Accédez à `/conf/wknd-mobile/settings/dam/cfm`.
   >    1. Supprimez la variable `models` node

   >    
   >Tous les modèles de fragment de contenu existants créés sous cette configuration sont supprimés, ainsi que leurs définitions, stockées sous . `/conf/wknd-mobile/settings/dam/cfm/models`.

1. Appliquez la variable **[!DNL WKND Mobile]** à la section **[!DNL WKND Mobile]Dossier de ressources** pour permettre la création de fragments de contenu à partir de modèles de fragment de contenu dans cette hiérarchie de dossiers de ressources :

   1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Ressources] > [!UICONTROL Fichiers]**
   1. Sélectionnez la **[!UICONTROL WKND Mobile] folder**
   1. Appuyez sur le bouton **[!UICONTROL Propriétés]** dans la barre d’actions supérieure pour ouvrir [!UICONTROL Propriétés du dossier]
   1. Dans [!UICONTROL Propriétés du dossier], appuyez sur le bouton **[!UICONTROL Cloud Services]** tab
   1. Vérifiez les **[!UICONTROL Configuration du cloud]** est défini sur **/conf/wknd-mobile**
   1. Appuyer **[!UICONTROL Enregistrer et fermer]** dans le coin supérieur droit pour conserver les modifications

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __Modèles de fragment de contenu__ ont été déplacés depuis __Outils > Ressources__ to __Outils > Général__.

## Présentation du modèle de fragment de contenu à créer

Avant de définir notre modèle de fragment de contenu, examinons l’expérience que nous allons créer pour nous assurer que nous capturons tous les points de données nécessaires. Pour ce faire, nous allons passer en revue la conception des applications mobiles et mapper les éléments de conception au contenu à collecter.

Nous pouvons ventiler les points de données qui définissent un événement comme suit :

![Création du modèle de fragment de contenu](assets/chapter-2/design-to-model-mapping.png)

Armé du mappage, nous pouvons définir le fragment de contenu utilisé pour collecter et exposer les données d’événement.

## Création du modèle de fragment de contenu

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles de fragment de contenu]**.
1. Appuyez sur le bouton **[!DNL WKND Mobile]** à ouvrir.
1. Appuyer **[!UICONTROL Créer]** pour ouvrir l’assistant de création d’un modèle de fragment de contenu.
1. Entrée **[!DNL Event]** comme la propriété **[!UICONTROL Titre du modèle]** *(description facultative)* et appuyez sur **[!UICONTROL Créer]** pour enregistrer.

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## Définition de la structure du modèle de fragment de contenu

1. Accédez à **[!UICONTROL Outils] > [!UICONTROL Général] > [!UICONTROL Modèles de fragment de contenu] >[!DNL WKND]**.
1. Sélectionnez la **[!DNL Event]** Modèle de fragment de contenu et appuyez sur **[!UICONTROL Modifier]** dans la barre d’actions supérieure.
1. Dans la **[!UICONTROL Types de données] tab** sur la droite, faites glisser le **[!UICONTROL Saisie de texte sur une seule ligne]** dans la zone de dépôt de gauche pour définir la variable **[!DNL Question]** champ .
1. Assurez-vous que la nouvelle **[!UICONTROL Saisie de texte sur une seule ligne]** est sélectionné à gauche, et la variable **[!UICONTROL Propriétés] tab** est sélectionné à droite. Renseignez les champs Propriétés comme suit :

   * [!UICONTROL Afficher comme] : `textfield`
   * [!UICONTROL Libellé du champ] : `Event Title`
   * [!UICONTROL Nom de la propriété] : `eventTitle`
   * [!UICONTROL Longueur max.] : 25
   * [!UICONTROL Requis] : `Yes`

Répétez ces étapes en utilisant les définitions d’entrée définies ci-dessous pour créer le reste du modèle de fragment de contenu d’événement.

>[!NOTE]
>
> Le **Nom de la propriété** Les champs DOIVENT correspondre exactement, car l’application Android est programmée pour retirer ces noms de la touche.

### Description d’événement

* [!UICONTROL Type de données] : `Multi-line text`
* [!UICONTROL Libellé du champ] : `Event Description`
* [!UICONTROL Nom de la propriété] : `eventDescription`
* [!UICONTROL Type par défaut] : `Rich text`

### Date et heure de l’événement

* [!UICONTROL Type de données] : `Date and time`
* [!UICONTROL Libellé du champ] : `Event Date and Time`
* [!UICONTROL Nom de la propriété] : `eventDateAndTime`
* [!UICONTROL Requis] : `Yes`

### Type d’événement

* [!UICONTROL Type de données] : `Enumeration`
* [!UICONTROL Libellé du champ] : `Event Type`
* [!UICONTROL Nom de la propriété] : `eventType`
* [!UICONTROL Options] : `Art,Music,Performance,Photography`

### Prix du ticket

* [!UICONTROL Type de données] : `Number`
* [!UICONTROL Afficher comme] : `numberfield`
* [!UICONTROL Libellé du champ] : `Ticket Price`
* [!UICONTROL Nom de la propriété] : `eventPrice`
* [!UICONTROL Type] : `Integer`
* [!UICONTROL Requis] : `Yes`

### Image d’événement

* [!UICONTROL Type de données] : `Content Reference`
* [!UICONTROL Afficher comme] : `contentreference`
* [!UICONTROL Libellé du champ] : `Event Image`
* [!UICONTROL Nom de la propriété] : `eventImage`
* [!UICONTROL Chemin d’accès racine] : `/content/dam/wknd-mobile/images`
* [!UICONTROL Requis] : `Yes`

### Nom du lieu

* [!UICONTROL Type de données] : `Single-line text`
* [!UICONTROL Afficher comme] : `textfield`
* [!UICONTROL Libellé du champ] : `Venue Name`
* [!UICONTROL Nom de la propriété] : `venueName`
* [!UICONTROL Longueur max.] : 20
* [!UICONTROL Requis] : `Yes`

### Ville de Venue

* [!UICONTROL Type de données] : `Enumeration`
* [!UICONTROL Libellé du champ] : `Venue City`
* [!UICONTROL Nom de la propriété] : `venueCity`
* [!UICONTROL Options] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>Le **[!UICONTROL Nom de la propriété]** indique le **both** le nom de la propriété JCR où cette valeur est stockée, ainsi que la clé dans le fichier JSON . Il doit s’agir d’un nom sémantique qui ne changera pas au cours de la durée de vie du modèle de fragment de contenu.

Après avoir créé le modèle de fragment de contenu, vous devez obtenir une définition qui ressemble à celle-ci :


![Modèle de fragment de contenu d’événement](assets/chapter-2/event-content-fragment-model.png)

## Étape suivante

Si vous le souhaitez, vous pouvez installer la variable [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) module de contenu sur l’auteur AEM via [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans cette partie du tutoriel.

* [Chapitre 3 - Création de fragments de contenu d’événement](./chapter-3.md)
