---
title: Configuration des règles de traduction dans AEM
description: L’interface utilisateur de configuration de traduction permet à un utilisateur de gérer des règles de traduction de contenu dans AEM Sites. Cette vidéo présente la création d’une règle de traduction pour un composant personnalisé.
feature: Copie de la langue
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localisation
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 7%

---


# Configuration des règles de traduction {#set-up-translation-rules-in-aem}

L’interface utilisateur de configuration de traduction permet à un utilisateur de gérer des règles de traduction de contenu dans AEM Sites. Cette vidéo présente la création d’une règle de traduction pour un composant personnalisé.

>[!NOTE]
>
> La vidéo ci-dessous a été enregistrée sur AEM 6.3. AEM 6.4+ présente une nouvelle structure de référentiel pour le stockage du fichier XML des règles de traduction. Lors de l’utilisation de l’interface utilisateur de configuration de traduction dans AEM version 6.4+, les règles sont enregistrées à l’emplacement `/conf/global/settings/translation/rules/translation_rules.xml`. Voir [Identification du contenu à traduire](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) pour plus d’informations.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

Les règles de traduction identifient le contenu de AEM à extraire pour traduction. Les règles de traduction prêtes à l’emploi couvrent les cas d’utilisation courants tels que les composants Texte et le texte de remplacement des composants Image. En fonction des exigences de traduction d’un projet, des règles supplémentaires peuvent être nécessaires. En règle générale, les règles de traduction permettent aux utilisateurs de spécifier :

1. Propriétés qui doivent être traduites en fonction du chemin et/ou du type de ressource
2. Filtres des propriétés qui ne doivent PAS être traduites
3. Contenu référencé qui doit être traduit (c’est-à-dire des images ou des fragments de contenu)

L’éditeur de règles de traduction qui mettra à jour le fichier XML de traduction. L’interface utilisateur de configuration de traduction facilite la gestion des différentes règles de traduction et des différents gardes contre les fautes de frappe lors de la modification directe du fichier XML.

Accédez à l’interface utilisateur de configuration de traduction :

* **[!UICONTROL AEM menu Démarrer]  >  [!UICONTROL Outils]  >  [!UICONTROL Général]  >  [[!UICONTROL Configuration de traduction]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Avant AEM 6.3 {#prior-to-aem}

Dans AEM version précédente, les règles de traduction étaient mises à jour manuellement en modifiant un fichier XML situé sous le workflow de traduction : `/etc/workflow/models/translation/translation_rules.xml`.

## Ressources supplémentaires {#additional-resources}

* [Identification du contenu à traduire](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traduction de contenu pour les sites multilingues](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Meilleures pratiques de traduction](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
