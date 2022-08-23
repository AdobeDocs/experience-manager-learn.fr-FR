---
title: Améliorations de la traduction dans AEM
description: AEM structure de traduction robuste permet AEM contenu d’être traduit en toute simplicité par les fournisseurs de traduction pris en charge. Découvrez les dernières améliorations.
version: 6.4, 6.5
topic: Localization
feature: Multi Site Manager, Language Copy
role: User
level: Beginner
exl-id: 21633308-ffe4-4023-affe-59269504da69
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 5%

---

# Améliorations de la traduction avec le gestionnaire multisite {#translation-enhancements}

AEM structure de traduction robuste permet AEM contenu d’être traduit en toute simplicité par les fournisseurs de traduction pris en charge.

## Améliorations apportées à la traduction dans AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=9&learn=on)

AEM version 6.5 de la traduction comprend :

**Approbation automatique des tâches de traduction**: L’indicateur d’approbation dans la tâche de traduction est une propriété binaire. Il ne génère pas ou ne s’intègre pas aux workflows de révision et d’approbation prêts à l’emploi. Pour réduire au minimum le nombre d’étapes d’une tâche de traduction, il est défini par défaut sur &quot;approbation automatique&quot; dans [!UICONTROL Propriétés avancées] d’un projet de traduction. Si votre entreprise nécessite l’approbation d’une tâche de traduction, vous pouvez décocher l’option &quot;Approbation automatique&quot; dans [!UICONTROL Propriétés avancées] d’un projet de traduction.

**Suppression automatique des lancements de traduction**: Plutôt que de supprimer manuellement les lancements de traduction dans l’administrateur Lancements, il est désormais possible de supprimer automatiquement les lancements de traduction une fois qu’ils ont été promus.

**Exportation d’objets de traduction au format JSON**: AEM version 6.4 et antérieure prennent en charge les formats XML et XLIFF des objets de traduction. Vous pouvez maintenant configurer le format d’exportation au format JSON à l’aide de la console de vos systèmes. [!UICONTROL Configuration Manager]. Rechercher [!UICONTROL Configuration de la plateforme de traduction], puis vous pouvez sélectionner le format d’exportation au format JSON.

**Mise à jour du contenu AEM traduit dans la mémoire de traduction (TMS)**: l’auteur local qui n’a pas accès à AEM peut apporter des mises à jour au contenu traduit, qui a déjà été ingéré dans AEM, directement dans la gestion dynamique des balises (mémoire de traduction, dans TMS), et mettre à jour les traductions dans l’ en renvoyant la tâche de traduction de TMS à.

## Améliorations de la traduction dans AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=9&learn=on)

Les auteurs peuvent désormais créer rapidement et facilement des projets de traduction multilingue directement depuis l’administrateur Sites ou l’administrateur Projets, configurer ces projets pour promouvoir automatiquement les lancements et même définir des calendriers d’automatisation.

## Ressources supplémentaires {#additional-resources}

* [Traduction de contenu pour les sites multilingues](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Bonnes pratiques de traduction](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
