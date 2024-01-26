---
title: Accélérer la vitesse du contenu avec les systèmes de style AEM
description: Découvrez comment utiliser les systèmes de style d’AEM pour permettre aux personnes travaillant dans la conception, la création de contenu et le développement dans votre organisation de créer et de diffuser des expériences à la vitesse et à l’échelle attendues par votre clientèle.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
duration: 216
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 100%

---

# Accélérer la vitesse du contenu avec les systèmes de style AEM

Dans cet article, vous découvrez comment utiliser les systèmes de style d’AEM pour permettre aux personnes travaillant dans la conception, la création de contenu et le développement dans votre organisation de créer et de diffuser des expériences à la vitesse et à l’échelle attendues par votre clientèle.

## Présentation

Les systèmes de style d’AEM présentent quatre avantages principaux :

* Les personnes créant des modèles peuvent définir des classes de style dans la stratégie de contenu d’un composant ou d’une page.
* Les personnes créant du contenu peuvent sélectionner des styles à appliquer à une page entière ou lors de la modification d’un composant sur une page.
* Les composants et les modèles sont plus flexibles et permettent aux personnes créatrices de proposer des variations visuelles alternatives.
* La nécessité de développer un composant personnalisé et/ou des boîtes de dialogue complexes pour présenter des variations de composant est réduite voire éliminée complètement.

## Configuration initiale et utilisation

La configuration en 5 étapes est très similaire à un workflow de développement de composant standard.

| **Direction** | **Designer** | **Équipe de développement et d’architecture** | **Créateur de modèles** | **Créateur de contenu** |
| --- | --- | --- | --- | --- |
| Détermine le contenu et les objectifs de ce composant. | Détermine la présentation visuelle et expérimentale du contenu. | Développe le CSS et le JS pour alimenter l’expérience ; définit et fournit des noms de classe à utiliser. | Configure les stratégies de modèle pour les composants stylisés en ajoutant les noms de classe CSS définis par l’équipe de développement. Des noms conviviaux doivent être utilisés pour chaque style. | Lors de la création de pages, applique les styles selon les besoins pour obtenir l’aspect souhaité. |

Bien qu’il s’agisse de la configuration initiale, de nombreux clientes et clients obtiennent plus d’agilité en rationalisant ce processus, par exemple en chargeant leur page CSS dans la gestion des ressources numériques (DAM), ce qui permet de mettre à jour les styles sans avoir à effectuer de déploiement. D’autres clientes et clients disposent d’un ensemble complet de classes d’utilitaires, ce qui leur permet de développer des composants et des styles qui peuvent ensuite être utilisés sans déploiement ni développement.

Les systèmes de style se présentent sous des formes différentes :

1. Styles de disposition

   * Modifications à plusieurs facettes de la conception et de la disposition.

   * Utilisés pour un rendu bien défini et identifiable.

1. Styles d’affichage
   * Variations mineures qui ne modifient pas la nature fondamentale du style.

   * Par exemple, la modification du modèle de couleurs, de la police, de l’orientation de l’image, etc.

1. Styles d’information

   * Affichent/masquent les champs.

>[!NOTE]
>
>Pour une démonstration de ces fonctionnalités, nous vous recommandons de regarder notre [Webinaire consacré au Succès client](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) avec Will Brisbane et Joseph Van Buskirk.

## Bonnes pratiques

* Donnez la priorité au style par défaut.
   * La disposition et l’affichage du composant déposé sur la page passent avant l’application des systèmes de style.
   * Il doit s’agir du rendu le plus utilisé.
* Dans la mesure du possible, essayez d’afficher uniquement les options de style qui ont un effet.
   * Si des combinaisons inefficaces sont exposées, assurez-vous qu’elles ne provoquent pas d’effets négatifs.
   * Par exemple, un style de disposition déterminant la position de l’image et accompagné d’un style d’affichage inefficace qui contrôle la position de l’image.
* Préférez les styles de disposition à une combinaison de styles d’affichage.
   * Cela réduit le nombre de permutations qui doivent être vérifiées en termes de qualité.
   * Cela garantit le respect des normes de marque.
   * Cela simplifie la création pour les personnes créant du contenu.
   * Cela permet de créer une identité de marque cohérente sur le site.
* Maniez les combinaisons de styles avec prudence.
   * À la fois entre les catégories et au sein des catégories.
* Consacrez le temps qu’il faut pour tester minutieusement les combinaison de styles.
   * Cela permet d’éviter les effets indésirables.
* Minimisez le nombre d’options et de permutations de style.
   * Un nombre trop important d’options peut entraîner un manque de cohérence dans l’aspect de la marque.
   * Cela peut créer de la confusion pour les personnes créant du contenu quant aux combinaisons nécessaires pour obtenir l’effet souhaité.
   * Cela augmente les permutations qui doivent être vérifiées en termes de qualité.
* Utilisez des libellés et des catégories de style conviviaux pour les personnes professionnelles.
   * « Bleu » et « Rouge » au lieu de « Primaire » et « Secondaire ».
   * « Carte » et « Héros » au lieu de « Variation A » et « Variation B ».
   * Il peut s’agir d’une banalité pour certains clientes et clients, mais l’équipe de conception, l’équipe commerciale et celle chargée du contenu connaissent très bien leurs couleurs primaires et secondaires ou les variations qu’elles testent. Mais dans un but de flexibilité et de prévision de potentielles modifications futures, l’utilisation de termes spécifiques peut être plus efficace.

## Principaux points à retenir

Les systèmes de style permettent de réduire le recours à des boîtes de dialogues complexes, mais ils ne les remplacent pas. Ils permettent de faciliter les opérations, mais il est possible que, dans certains cas, vous souhaitiez utiliser les propriétés ou la boîte de dialogue du composant plutôt que de créer un système de style pour celui-ci.

Ils peuvent rationaliser les processus du point de vue du développement. Vous pouvez obtenir plusieurs styles du même contenu avec un seul système de style. De même, du point de vue de la création, il est possible d’accélérer le processus de création plutôt que de former les créateurs et créatrices et de les obliger à se rappeler quel composant utiliser à quel endroit.

Les choses sont tout simplement plus épurées. Le HTML au sein des composants principaux est très détaillé. Le fait de réaliser tout cela au niveau du CSS accélère la construction du composant et permet d’avoir un code plus épuré.

Pour finir, l’utilisation des systèmes de style relève plus de l’art que de la science. Comme nous l’avons vu, il y a plusieurs bonnes pratiques, mais vous aurez toute liberté pour personnaliser la configuration de votre organisation.

Pour plus d’informations, consultez notre [Webinaire sur le succès client](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) avec Will Brisbane et Joseph Van Buskirk.

Pour en savoir plus sur la stratégie et le leadership de la pensée, consultez le hub [Succès client](https://experienceleague.adobe.com/docs/customer-success/customer-success/overview.html?lang=fr).
