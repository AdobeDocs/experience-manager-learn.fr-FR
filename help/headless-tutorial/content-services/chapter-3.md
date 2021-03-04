---
title: Chapitre 3 - Création de fragments de contenu de Événement - Content Services
seo-title: Prise en main de AEM Content Services - Chapitre 3 - Création de fragments de contenu de Événement
description: Le chapitre 3 du didacticiel AEM sans en-tête couvre la création et la création de fragments de contenu de Événement à partir du modèle de fragment de contenu créé dans le chapitre 2.
seo-description: Le chapitre 3 du didacticiel AEM sans en-tête couvre la création et la création de fragments de contenu de Événement à partir du modèle de fragment de contenu créé dans le chapitre 2.
feature: '"Fragments de contenu, API"'
topic: '"Sans tête, Gestion de contenu"'
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 4%

---


# Chapitre 3 - Création de fragments de contenu de Événement

Le chapitre 3 du didacticiel AEM sans en-tête couvre la création et la création de fragments de contenu de Événements à partir du modèle de fragment de contenu créé dans [Chapitre 2](./chapter-2.md).

## Création d’un fragment de contenu de Événement

Avec un modèle de fragment de contenu [!DNL Event] créé et la configuration AEM pour WKND appliquée au dossier `/content/dam/wknd-mobile` Actif (via la propriété `cq:conf`), un fragment de contenu [!DNL Event] peut être créé.

Les fragments de contenu, qui sont un type de ressource, doivent être organisés et gérés en AEM Assets, tout comme les autres ressources.

* Utiliser les dossiers de paramètres régionaux dans la structure de dossiers Ressources si la traduction est (ou peut être) requise
* Organisez logiquement les fragments de contenu pour faciliter leur localisation et leur gestion.

Dans cette étape, créez un nouveau [!DNL Event] pour `Punkrock Fest` dans le dossier `/content/dam/wknd-mobile/en/events` ressources.

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

1. A l’aide de [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp), installez le package ci-dessous sur AEM Author. Ce package contient plusieurs fragments de contenu Événement.

   [Obtenir le fichier : GitHub > Ressources > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## Vérification de la structure JCR du fragment de contenu

*Cette section est uniquement à titre d’information et vise à socialiser la structure JCR sous-jacente des fragments de contenu créés à partir de modèles de fragments de contenu.*

1. Ouvrez **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** sur AEM Author.
1. Dans le CRXDE Lite, dans le menu de la hiérarchie de gauche, accédez à [/content/dam/wknd-mobile/en/événements/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) qui est le noeud représentant le fragment de contenu [!DNL Punkrock Fest] [!DNL Event] dans le JCR.
1. Développez le noeud [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master).
Vérifiez dans le volet **Propriétés** qu&#39;il possède une propriété `cq:model` qui pointe vers la définition de modèle de fragment de contenu [!DNL Event].
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. Sous le noeud `data`, sélectionnez le noeud [maître](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) et passez en revue les propriétés. Ce noeud contient le contenu collecté lors de la création d&#39;un modèle de fragment de contenu [!DNL Event]. Les noms de propriété JCR correspondent aux noms de propriété Modèle de fragment de contenu et les valeurs correspondent aux valeurs créées du fragment de contenu &quot;[!DNL Punkrock Fest]&quot; [!DNL Event].

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## Étape suivante

Il est recommandé d’installer le package de contenu [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) sur AEM Author via [AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp). Ce package contient les configurations et le contenu décrits dans ce didacticiel et les chapitres précédents.

* [Chapitre 4 - Définition des modèles AEM Content Services](./chapter-4.md)
