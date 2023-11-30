---
title: Configurer des règles de traduction dans AEM
description: L’interface utilisateur de configuration de la traduction permet à un utilisateur ou une utilisatrice de gérer des règles de traduction de contenu dans AEM Sites. Cette vidéo présente la création d’une règle de traduction pour un composant personnalisé.
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 100%

---

# Configurer les règles de traduction {#set-up-translation-rules-in-aem}

L’interface utilisateur de configuration de la traduction permet à un utilisateur ou une utilisatrice de gérer des règles de traduction de contenu dans AEM Sites. Cette vidéo présente la création d’une règle de traduction pour un composant personnalisé.

>[!NOTE]
>
> La vidéo ci-dessous a été enregistrée sur AEM 6.3. AEM 6.4 et ultérieure introduit une nouvelle structure de référentiel pour le stockage du fichier XML des règles de traduction. Lorsque vous utilisez l’interface utilisateur de configuration de la traduction dans AEM 6.4 et ultérieure, les règles sont enregistrées à l’emplacement `/conf/global/settings/translation/rules/translation_rules.xml`. Consultez [Identification du contenu à traduire](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-rules.html?lang=fr) pour plus d’informations.

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

Les règles de traduction identifient le contenu de AEM à extraire pour traduction. Les règles de traduction prêtes à l’emploi couvrent les cas d’utilisation courants tels que les composants Texte et le texte de remplacement des composants d’image. En fonction des exigences de traduction d’un projet, des règles supplémentaires peuvent s’avérer nécessaires. En règle générale, les règles de traduction permettent aux utilisateurs et aux utilisatrices de spécifier :

1. les propriétés qui doivent être traduites en fonction du chemin et/ou du type de ressource ;
2. les filtres pour les propriétés qui ne doivent PAS être traduites ;
3. le contenu référencé à traduire (c’est-à-dire des images ou des fragments de contenu) ;

l’éditeur de règles de traduction qui mettra à jour le fichier XML de traduction. L’interface utilisateur de configuration de la traduction facilite la gestion des différentes règles de traduction et permet d’éviter les fautes de frappe lors de la modification directe du fichier XML.

Accédez à l’interface utilisateur de configuration de la traduction :

* **[!UICONTROL Menu AEM] > [!UICONTROL Outils] > [!UICONTROL Général] > [[!UICONTROL Configuration de la traduction]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**.

## Avant la version 6.3 d’AEM {#prior-to-aem}

Dans la version précédente d’AEM, les règles de traduction étaient mises à jour manuellement en modifiant un fichier XML situé sous le workflow de traduction : `/etc/workflow/models/translation/translation_rules.xml`.

## Ressources supplémentaires {#additional-resources}

* [Identification du contenu à traduire](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-rules.html?lang=fr)
* [Traduction de contenu pour les sites multilingues](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/translation.html)
* [https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-manage.html?lang=fr](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-manage.html?lang=fr)
* [Bonnes pratiques de traduction](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-bp.html?lang=fr)
