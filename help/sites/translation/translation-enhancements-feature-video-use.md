---
title: Améliorations apportées à la traduction dans AEM
description: Le framework de traduction robuste d’AEM permet aux fournisseurs de traduction pris en charge de traduire du contenu AEM en toute simplicité. Découvrez les dernières améliorations apportées.
version: 6.4, 6.5
topic: Localization
feature: Multi Site Manager, Language Copy
role: User
level: Beginner
doc-type: Feature Video
exl-id: 21633308-ffe4-4023-affe-59269504da69
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 100%

---

# Améliorations de la traduction avec Multi-Site Manager {#translation-enhancements}

Le framework de traduction robuste d’AEM permet aux fournisseurs de traduction pris en charge de traduire du contenu AEM en toute simplicité.

## Améliorations apportées à la traduction dans AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=12&learn=on)

Les améliorations apportées à la traduction d’AEM 6.5 incluent les éléments suivants :

**Approbation automatique des tâches de traduction** : l’indicateur d’approbation de la tâche de traduction est une propriété binaire. Il ne génère pas de workflows de révision et d’approbation prêts à l’emploi et ne s’intègre pas à ces derniers. Pour réduire au minimum le nombre d’étapes d’une tâche de traduction, l’option « Approbation automatique » est choisie par défaut dans les [!UICONTROL Propriétés avancées] des projets de traduction. Si votre organisation nécessite l’approbation d’une tâche de traduction, vous pouvez décocher l’option « Approbation automatique » dans les [!UICONTROL Propriétés avancées] des projets de traduction.

**Suppression automatique des lancements de traduction** : au lieu de supprimer manuellement et après coup les lancements de traduction dans l’administration des lancements, il est désormais possible de les supprimer automatiquement après leur conversion.

**Export d’objets de traduction au format JSON** : AEM 6.4 et les versions antérieures prennent en charge les formats XML et XLIFF des objets de traduction. Vous pouvez maintenant configurer le format d’export au format JSON à l’aide de votre console système [!UICONTROL Config Manager]. Recherchez [!UICONTROL Configuration de la plateforme de traduction], puis sélectionnez le format d’export JSON.

**Mise à jour du contenu AEM traduit dans la mémoire de traduction (TMS)** : la personne locale chargée de la création qui n’a pas accès à AEM peut apporter des mises à jour au contenu traduit déjà ingéré dans AEM, c’est-à-dire directement dans la TM (mémoire de traduction, dans TMS), puis mettre à jour les traductions dans AEM en renvoyant la tâche de traduction de TMS vers AEM

## Améliorations apportées à la traduction dans AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=12&learn=on)

Les auteurs et autrices peuvent désormais créer rapidement et facilement des projets de traduction multilingues directement depuis l’administration Sites ou l’administration Projets, configurer ces projets pour convertir automatiquement les lancements et même définir des calendriers d’automatisation.

## Ressources supplémentaires {#additional-resources}

* [Traduction de contenu pour les sites multilingues](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/translation.html)
* [https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-manage.html?lang=fr](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-manage.html?lang=fr)
* [Bonnes pratiques de traduction](https://experienceleague.adobe.com/docs/experience-manager-65/administering/introduction/tc-bp.html?lang=fr)
