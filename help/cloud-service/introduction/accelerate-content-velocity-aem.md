---
title: Accélérer la vitesse du contenu avec les systèmes de style AEM
description: Découvrez comment utiliser AEM système de style pour permettre aux concepteurs, aux auteurs de contenu et aux développeurs de votre entreprise de créer et de diffuser des expériences à la vitesse et à l’échelle attendues par vos clients.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 0%

---

# Accélérer la vitesse du contenu avec les systèmes de style AEM

Dans cet article, vous découvrez comment utiliser des systèmes de type AEM pour permettre aux concepteurs, aux auteurs de contenu et aux développeurs de votre entreprise de créer et de diffuser des expériences à la vitesse et à l’échelle attendues par vos clients.

##  du commerce électronique

Les systèmes de style AEM présentent quatre avantages principaux :

* Les auteurs de modèles peuvent définir des classes de style dans la stratégie de contenu d’un composant ou d’une page.
* Les auteurs de contenu peuvent sélectionner des styles à appliquer à une page entière ou lors de la modification d’un composant sur une page.
* Les composants et les modèles sont plus flexibles en permettant aux auteurs de rendre des variantes visuelles de remplacement.
* La nécessité de développer un composant personnalisé et/ou des boîtes de dialogue complexes pour présenter des variations de composant est complètement réduite ou éliminée.

## Configuration initiale et utilisation

La configuration en 5 étapes est très similaire à un workflow de développement de composant standard.

| **Leadership** | **Designer** | **Développeur/Architecte** | **Créateur de modèles** | **Auteur de contenu** |
| --- | --- | --- | --- | --- |
| Détermine le contenu et les objectifs de ce composant | Détermine la présentation visuelle et expérientielle du contenu | Développe CSS et JS pour prendre en charge l’expérience ; définit et fournit des noms de classe à utiliser ; | Configure les stratégies de modèle pour les composants stylisés en ajoutant les noms de classe CSS définis par les développeurs. Les noms conviviaux doivent être utilisés pour chaque style. | Lors de la création de pages, applique les styles selon les besoins pour obtenir l’aspect souhaité. |

Bien qu’il s’agisse de la configuration initiale, de nombreux clients ont plus d’agilité en rationalisant ce processus, par exemple en chargeant leur page CSS dans la gestion des ressources numériques, ce qui permet de mettre à jour les styles sans avoir à effectuer de déploiement. D’autres clients disposent d’un ensemble complet de classes d’utilitaires, ce qui leur permet de développer des composants et des styles qui peuvent ensuite être utilisés sans déploiement ni développement.

Les systèmes de style se présentent sous des formes différentes :

1. Styles de disposition

   * Modifications à plusieurs facettes de la conception et de la mise en page

   * Utilisé pour le rendu bien défini et identifiable

1. Styles d’affichage
   * Variations mineures qui ne modifient pas la nature fondamentale du style

   * Par exemple, modifier le modèle de couleurs, la police, l’orientation de l’image, etc.

1. Styles d’information

   * Afficher/masquer les champs

>[!NOTE]
>
>Pour une démonstration de ces fonctionnalités, nous vous recommandons de regarder notre [Webinaire sur la réussite client](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) avec Will Brisbane et Joseph Van Buskirk.

## Bonnes pratiques

* Donner la priorité au style par défaut
   * Disposition et affichage du composant lorsqu’il est déposé sur la page avant l’application de systèmes de style
   * Il doit s’agir du rendu le plus utilisé.
* Essayez d’afficher uniquement les options de style qui ont un effet lorsque cela est possible.
   * Si des combinaisons inefficaces sont exposées, assurez-vous qu’elles ne provoquent pas d’effets négatifs.
   * Par exemple, un style de mise en page qui détermine la position de l’image et est accompagné d’un style d’affichage inefficace qui contrôle la position de l’image.
* Opt-on pour les styles de mise en page par rapport aux styles d’affichage combinés
   * Réduit le nombre de permutations qui doivent être vérifiées en termes de qualité.
   * Garantit le respect des normes de marque
   * Simplification de la création pour les auteurs de contenu
   * Permet de créer une identité de marque cohérente sur le site
* Soyez prudent avec les styles combinés
   * À la fois dans les catégories
* Attribuer le temps approprié pour tester minutieusement les styles combinés
   * Permet d’éviter les effets indésirables
* Minimiser le nombre d’options de style et de permutations
   * Trop d’options peut entraîner un manque de cohérence de la marque pour l’aspect
   * Peut créer de la confusion pour les auteurs de contenu sur les combinaisons nécessaires pour obtenir l’effet souhaité.
   * Augmente les permutations qui doivent être vérifiées en termes de qualité
* Utilisation des libellés et des catégories de style conviviaux pour les entreprises
   * &quot;Bleu&quot; et &quot;Rouge&quot; au lieu de &quot;Principal&quot; et &quot;Secondaire&quot;
   * &quot;Carte&quot; et &quot;Héros&quot; au lieu de &quot;Variation A&quot; et &quot;Variation B&quot;
   * Cela peut être plus général pour certains clients ; l’équipe de conception, l’équipe d’entreprise et l’équipe de contenu connaissent bien leurs couleurs Principales et secondaires ou les variations qu’elles testent. Mais pour la flexibilité et tout potentiel de changements futurs, l&#39;utilisation de termes spécifiques peut être plus efficace.

## Principales acquisitions

Les systèmes de style réduisent le besoin de boîtes de dialogue complexes, mais il ne s’agit pas d’un remplacement de boîte de dialogue. Ils aident à simplifier les choses, mais il peut arriver que vous souhaitiez utiliser des propriétés de composant ou une boîte de dialogue plutôt que de créer un système de style pour celle-ci.

Ils peuvent rationaliser les processus du point de vue du développement. Vous pouvez obtenir plusieurs styles du même contenu avec un seul système de style. De même, du point de vue de la création, plutôt que de former les auteurs et les auteurs qui doivent se rappeler quel composant utiliser dans quel palais, vous pouvez accélérer la vitesse de création.

Les choses sont tout simplement plus propres. Le HTML au sein des composants principaux est très détaillé. Tout cela au niveau CSS rend le composant plus rapide et le code plus propre.

Enfin, l&#39;utilisation des systèmes de style est plus de l&#39;art que de la science. Comme nous l’avons expliqué, il existe plusieurs bonnes pratiques, mais vous disposez d’une certaine souplesse dans la personnalisation de la configuration de votre entreprise.

Pour plus d’informations, consultez notre [Webinaire sur le succès client](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) avec Will Brisbane et Joseph Van Buskirk.

Pour en savoir plus sur la stratégie et le leadership de la pensée, voir [Succès des clients](https://experienceleague.adobe.com/docs/customer-success/customer-success/overview.html) hub.
