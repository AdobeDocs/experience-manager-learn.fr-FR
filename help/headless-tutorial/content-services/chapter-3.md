---
title: Chapitre 3 - Création de fragments de contenu d’événement - Content Services
seo-title: Prise en main d’AEM Content Services - Chapitre 3 - Création de fragments de contenu d’événement
description: Le chapitre 3 du tutoriel AEM sans affichage porte sur la création et la création de fragments de contenu d’événement à partir du modèle de fragment de contenu créé dans le chapitre 2.
seo-description: Le chapitre 3 du tutoriel AEM sans affichage porte sur la création et la création de fragments de contenu d’événement à partir du modèle de fragment de contenu créé dans le chapitre 2.
feature: Fragments de contenu, API
topic: Sans affichage, gestion de contenu
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# Chapitre 3 - Création de fragments de contenu d’événement

Le chapitre 3 du tutoriel AEM sans affichage porte sur la création et la création de fragments de contenu d’événements à partir du modèle de fragment de contenu créé dans [Chapitre 2](./chapter-2.md).

## Création d’un fragment de contenu d’événement

Avec un [!DNL Event] modèle de fragment de contenu créé et la configuration d’AEM pour WKND appliquée au dossier `/content/dam/wknd-mobile` Ressource (via la propriété `cq:conf`), un fragment de contenu [!DNL Event] peut être créé.

Les fragments de contenu, qui sont un type de ressource, doivent être organisés et gérés dans AEM Assets comme les autres ressources.

* Utilisez des dossiers de paramètres régionaux dans la structure de dossiers de ressources si la traduction est (ou peut être) requise.
* Organisez de manière logique les fragments de contenu afin qu’ils soient faciles à localiser et à gérer

Au cours de cette étape, créez un [!DNL Event] pour `Punkrock Fest` dans le dossier `/content/dam/wknd-mobile/en/events` ressources.

1. Accédez à **[!UICONTROL AEM] > [!UICONTROL Ressources] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] >[!DNL English]** et créez des dossiers de ressources **[!DNL Events]**.
1. Dans **[!UICONTROL Ressources] > [!UICONTROL Fichiers] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** créez un fragment de contenu de type **[!DNL Event]** avec le titre **[!DNL Punkrock Fest]**.
1. Créez le fragment de contenu [!DNL Event] nouvellement créé.

   * [!DNL Event Title] : **[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] :  **Musique**
   * [!DNL Ticket Price] :  **10**
   * [!DNL Event Image] :  **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] :  **La Maison des reptiles**
   * [!DNL Venue City] : **New York**

   Appuyez sur **[!UICONTROL Enregistrer]** dans la barre d’actions supérieure pour enregistrer les modifications.

1. À l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp), installez le package ci-dessous sur AEM Author. Ce package contient un certain nombre de fragments de contenu d’événement.

   [Obtenir le fichier : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Vérification de la structure JCR du fragment de contenu

*Cette section est informative uniquement et a pour but de socialiser la structure JCR sous-jacente des fragments de contenu créés à partir de modèles de fragments de contenu.*

1. Ouvrez **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** sur l’auteur AEM.
1. Dans CRXDE Lite, dans le menu de hiérarchie de gauche, accédez à [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) qui est le noeud représentant le [!DNL Punkrock Fest] [!DNL Event] fragment de contenu dans le JCR.
1. Développez le noeud [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) .
Vérifiez dans le **volet Propriétés** qu’il possède une propriété `cq:model` pointant vers la définition de modèle de fragment de contenu [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sous le noeud `data` , sélectionnez le noeud [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) et vérifiez les propriétés. Ce noeud contient le contenu collecté lors de la création d’un modèle de fragment de contenu [!DNL Event]. Les noms des propriétés JCR correspondent aux noms des propriétés du modèle de fragment de contenu et les valeurs correspondent aux valeurs créées du fragment de contenu &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Étape suivante

Il est recommandé d’installer le module de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur l’auteur AEM via [AEM [!UICONTROL Gestionnaire de modules]](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce tutoriel et dans les chapitres précédents.

* [Chapitre 4 - Définition AEM modèles Content Services](./chapter-4.md)
