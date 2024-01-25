---
title: Utiliser la recherche de traduction intelligente avec AEM Assets
description: La recherche de traduction intelligente permet la recherche et la découverte multilingues automatiques dans le contenu AEM, y compris les ressources et les pages. Elle prend en charge plus de 50 langues et réduit, dès lors, le besoin de traduction manuelle du contenu.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 195
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 100%

---

# Utiliser la recherche de traduction intelligente avec AEM Assets{#using-smart-translation-search-with-aem-assets}

La recherche de traduction intelligente permet la recherche et la découverte multilingues automatiques dans le contenu AEM, y compris les ressources et les pages. Elle prend en charge plus de 50 langues et réduit, dès lors, le besoin de traduction manuelle du contenu.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

La recherche de traduction intelligente AEM permet de rechercher le contenu AEM dans une autre langue que celle de Shakespeare, afin de trouver les ressources d’AEM avec les termes anglais correspondants.

La recherche de traduction intelligente est le compagnon idéal des balises intelligentes AEM appliquées aux ressources en anglais.

Cette vidéo part du principe que vous avez déjà configuré la [recherche de traduction intelligente AEM](smart-translation-search-technical-video-setup.md).

## Fonctionnement de la recherche de traduction intelligente {#how-smart-translation-search-works}

![Diagramme de flux de la recherche de traduction intelligente.](assets/smart-translation-search-flow.png)

1. L’utilisateur ou l’utilisatrice d’AEM effectue une recherche en texte intégral et saisit un terme de recherche localisé (par exemple, le mot espagnol pour « homme », « hombre » (« man » en anglais)).
2. La recherche de traduction intelligente, fournie par le lot OSGi de traduction automatique Apache Oak, démarre et évalue si les termes de recherche saisis peuvent être traduits à l’aide des modules linguistiques enregistrés.
3. Tous les termes traduits de l’étape 2 sont collectés et la requête est enrichie en interne afin de les inclure comme termes de recherche. Ce jeu enrichi de termes de recherche est évalué normalement par rapport aux index de recherche d’AEM, ce qui permet de trouver des correspondances pertinentes.
4. Les résultats de la recherche qui correspondent au terme d’origine (« hombre ») ou au terme traduit (« man ») sont collectés et renvoyés à l’utilisateur ou l’utilisatrice comme résultats de la recherche.

## Ressources supplémentaires{#additional-resources}

* [Configurer la recherche de traduction intelligente avec AEM Assets](smart-translation-search-technical-video-setup.md)
* [Modules linguistiques Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
