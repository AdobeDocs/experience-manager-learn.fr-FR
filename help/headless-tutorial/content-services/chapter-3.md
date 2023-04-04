---
title: Chapitre 3 - Création de fragments de contenu d’événement - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: Le chapitre 3 du tutoriel AEM sans affichage porte sur la création et la création de fragments de contenu d’événement à partir du modèle de fragment de contenu créé dans le chapitre 2.
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 4%

---

# Chapitre 3 - Création de fragments de contenu d’événement

Le chapitre 3 du tutoriel AEM sans affichage porte sur la création et la création de fragments de contenu d’événements à partir du modèle de fragment de contenu créé dans [Chapitre 2](./chapter-2.md).

## Création d’un fragment de contenu d’événement

Avec [!DNL Event] Modèle de fragment de contenu créé et la configuration AEM pour WKND appliquée au `/content/dam/wknd-mobile` Dossier de ressources (via `cq:conf` ), a [!DNL Event] Il est possible de créer un fragment de contenu.

Les fragments de contenu, qui sont un type de ressource, doivent être organisés et gérés dans AEM Assets comme les autres ressources.

* Utilisez des dossiers de paramètres régionaux dans la structure de dossiers de ressources si la traduction est (ou peut être) requise.
* Organisez de manière logique les fragments de contenu afin qu’ils soient faciles à localiser et à gérer

Au cours de cette étape, créez une [!DNL Event] pour `Punkrock Fest` dans le `/content/dam/wknd-mobile/en/events` dossier assets .

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Ressources] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] >[!DNL English]** et créer des dossiers de ressources **[!DNL Events]**.
1. Within **[!UICONTROL Ressources] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** créer un fragment de contenu de type **[!DNL Event]** avec un titre de **[!DNL Punkrock Fest]**.
1. Créez le [!DNL Event] Fragment de contenu.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **Musique**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **La Maison des reptiles**
   * [!DNL Venue City] : **New York**

   Appuyer **[!UICONTROL Enregistrer]** dans la barre d’actions supérieure pour enregistrer les modifications.

1. Utilisation [AEM Gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp), installez le module ci-dessous sur AEM Author. Ce package contient un certain nombre de fragments de contenu d’événement.

   [Obtenir le fichier : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## Vérification de la structure JCR du fragment de contenu

*Cette section est informative uniquement et a pour but de socialiser la structure JCR sous-jacente des fragments de contenu créés à partir de modèles de fragments de contenu.*

1. Ouvrir **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** sur l’auteur AEM.
1. Dans CRXDE Lite, dans le menu de hiérarchie de gauche, accédez à [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) qui est le noeud représentant la propriété [!DNL Punkrock Fest] [!DNL Event] Fragment de contenu dans le JCR.
1. Développez l’objet [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) noeud .
Dans le **Volet Propriétés** qu’il possède une propriété `cq:model` qui pointe vers le [!DNL Event] Définition du modèle de fragment de contenu.
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sous la `data` sélectionnez le noeud [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) et passez en revue les propriétés. Ce noeud contient le contenu collecté lors de la création d’un [!DNL Event] Modèle de fragment de contenu. Les noms des propriétés JCR correspondent aux noms des propriétés du modèle de fragment de contenu, et les valeurs correspondent aux valeurs créées du &quot;[!DNL Punkrock Fest]&quot; [!DNL Event] Fragment de contenu.

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## Étape suivante

Il est recommandé d’installer le [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) module de contenu sur l’auteur AEM via [AEM [!UICONTROL Gestionnaire de modules]](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 4 - Définition AEM modèles Content Services](./chapter-4.md)
