---
title: Chapitre 3 - Création de fragments de contenu d’événement - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: Le chapitre 3 du tutoriel AEM Headless porte sur la création de fragments de contenu d’événement à partir du modèle de fragment de contenu créé dans le chapitre 2.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '478'
ht-degree: 100%

---

# Chapitre 3 - Création de fragments de contenu d’événement

Le chapitre 3 du tutoriel AEM Headless porte sur la création de fragments de contenu d’événements à partir du modèle de fragment de contenu créé dans le [chapitre 2](./chapter-2.md).

## Création d’un fragment de contenu d’événement

Avec un modèle de fragment de contenu [!DNL Event] créé et la configuration AEM pour WKND appliquée au dossier de ressources `/content/dam/wknd-mobile` (via la propriété `cq:conf`), il est possible de créer un fragment de contenu [!DNL Event].

Les fragments de contenu, qui sont un type de ressources, doivent être organisés et gérés dans AEM Assets comme les autres ressources.

* Utilisez des dossiers de paramètres régionaux dans la structure du dossier Assets si une traduction est (ou peut être) requise.
* Organisez de manière logique les fragments de contenu afin qu’ils soient faciles à localiser et à gérer.

Au cours de cette étape, nous allons créer un nouvel [!DNL Event] pour `Punkrock Fest` dans le dossier de ressources `/content/dam/wknd-mobile/en/events`.

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] >[!DNL English]** et créez des dossiers Assets **[!DNL Events]**.
1. Dans **[!UICONTROL Assets] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**, créez un fragment de contenu de type **[!DNL Event]** avec le titre **[!DNL Punkrock Fest]**.
1. Indiquez les paramètres du fragment de contenu [!DNL Event] nouvellement créé.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;Saisissez une description de quelques lignes...>**
   * [!DNL Event Date] : **&lt;Sélectionnez une date future>**
   * [!DNL Event Type] : **Musique**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La Maison des reptiles**
   * [!DNL Venue City] : **New York**

   Appuyez sur **[!UICONTROL Enregistrer]** dans la barre d’actions supérieure pour enregistrer les modifications.

1. À l’aide du [gestionnaire de modules AEM](http://localhost:4502/crx/packmgr/index.jsp), installez le module ci-dessous sur AEM Author. Ce module contient un certain nombre de fragments de contenu d’événement.

   [Obtenir le fichier : GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Vérification de la structure JCR du fragment de contenu

*Cette section est uniquement informative et a pour but de faire connaitre la structure JCR sous-jacente des fragments de contenu créés à partir de modèles de fragments de contenu.*

1. Ouvrez **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** sur AEM Author.
1. Dans CRXDE Lite, dans le menu hiérarchique de gauche, accédez à [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) qui est le nœud représentant le fragment de contenu [!DNL Event] [!DNL Punkrock Fest] dans le JCR.
1. Développez le nœud de [données](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Vérifiez dans le **volet Propriétés** qu’il possède une propriété `cq:model` pointant vers la définition du modèle de fragment de contenu [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sous le nœud `data`, sélectionnez le nœud [principal](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) et passez en revue les propriétés. Ce nœud contient le contenu collecté lors de la création d’un modèle de fragment de contenu [!DNL Event]. Les noms des propriétés JCR correspondent aux noms des propriétés du modèle de fragment de contenu, et les valeurs correspondent aux valeurs créées du fragment de contenu [!DNL Event] « [!DNL Punkrock Fest] ».

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Étape suivante

Il est recommandé d’installer le module de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur AEM Author via le [Gestionnaire de modules [!UICONTROL  AEM ]](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 4 - Définition des modèles AEM Content Services](./chapter-4.md)
