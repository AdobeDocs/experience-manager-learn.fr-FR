---
title: Prise en main d’AEM et d’Adobe Target
description: Tutoriel complet montrant comment créer et diffuser des expériences personnalisées à l’aide d’Adobe Experience Manager et d’Adobe Target. Dans ce tutoriel, vous découvrirez également différentes personnes impliquées dans le processus de bout en bout et comment elles collaborent entre elles.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# Prise en main d’AEM et d’Adobe Target {#getting-started-with-aem-target}

AEM et Target sont deux solutions puissantes avec des fonctionnalités qui semblent se chevaucher. Les clients ont parfois du mal à comprendre comment et quand utiliser ces produits conjointement pour offrir une expérience personnalisée. Pour offrir une expérience optimisée à chaque utilisateur final, différentes équipes de votre entreprise doivent travailler en étroite collaboration et définir qui fait quoi.

Dans ce tutoriel, nous présentons trois scénarios différents pour AEM et Target, qui vous aident à comprendre ce qui fonctionne le mieux pour votre organisation et comment les différentes équipes collaborent.

* Scénario 1 : Personnalisation à l’aide des fragments d’expérience AEM
* Scénario 2 : Personnalisation à l’aide du compositeur d’expérience visuelle
* Scénario 3 : Personnalisation des expériences de pages Web complètes

## Personnalisation à l’aide des fragments d’expérience AEM {#personalization-using-aem-experience-fragment}

Pour ce scénario, nous allons utiliser AEM et Target. De toute évidence, les deux produits ont leurs propres points forts et, lorsqu’il s’agit de fournir des expériences personnalisées aux utilisateurs de votre site, vous avez besoin de **contenu personnalisé (contenu d&#39;AEM)** et un **méthode intelligente (Target)** pour diffuser ces contenus en fonction d’un utilisateur spécifique.

AEM vous aide à créer du contenu personnalisé, rassemblant l’ensemble de vos contenus et ressources à un emplacement central afin d’alimenter votre stratégie de personnalisation. AEM permet de créer facilement du contenu pour les ordinateurs de bureau, les tablettes et les appareils mobiles à un seul endroit sans devoir écrire de code. Il n’est pas nécessaire de créer des pages pour chaque appareil : AEM ajuste automatiquement chaque expérience à l’aide de votre contenu. Vous pouvez également exporter le contenu d’AEM vers Adobe Target sous forme d’offres en appuyant sur un bouton.

Nous disposons désormais d’un contenu personnalisé sous la forme d’offres d’AEM dans Target. Target vous permet de diffuser ces offres à grande échelle sur la base d’une combinaison d’approches d’apprentissage automatique basées sur des règles et pilotées par l’IA qui intègrent des variables comportementales, contextuelles et hors ligne.  Avec Target, vous pouvez facilement configurer et exécuter des activités A/B et multivariées (MVT) afin de déterminer les meilleures offres, contenus et expériences.

**Fragments d’expérience** représentent un énorme pas en avant pour lier les créateurs de contenu/d’expérience aux professionnels de la personnalisation qui génèrent des résultats commerciaux à l’aide de Target.

* AEM les auteurs de contenu personnalisé en tant que fragments d’expérience et leurs variantes
* AEM exporte le HTML de fragment d’expérience vers Target &#x200B;
* Target &#x200B; utilise AEM balisage de fragment d’expérience comme offres dans les activités
* Target diffuse le HTML de fragment d’expérience, AEM fournit des images référencées.

   ![Personnalisation à l’aide du diagramme Fragments d’expérience](assets/personalization-use-case-1/use-case-1-diagram.png)

**Pour mettre en oeuvre ce scénario, vous devez :**

* [Intégration d’AEM et d’Adobe Target à l’aide de Launch et d’Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM et Adobe Target à l’aide de Cloud Services hérités](./implementation.md#integrating-aem-target-options)

***Après la mise en oeuvre des intégrations ci-dessus, nous vous proposons d’explorer la variable [scénario en détail](./personalization-use-case-1.md).***

## Personnalisation à l’aide du compositeur d’expérience visuelle

Les marketeurs peuvent apporter des modifications rapides à leur site web sans modifier de code pour exécuter un test à l’aide du compositeur d’expérience visuelle (VEC) d’Adobe Target. Le compositeur d’expérience visuelle est une interface utilisateur WYSIWYG (ce que vous voyez est ce que vous obtenez) qui vous permet de créer et de tester facilement des expériences et des offres personnalisées dans le contexte du site. Vous pouvez créer des expériences et des offres pour les activités Target en faisant glisser, en permutant et en modifiant la mise en page et le contenu d’une page web (ou d’une offre) ou d’une page web mobile.

Le compositeur d’expérience visuelle est l’une des principales fonctionnalités d’Adobe Target. Le compositeur d’expérience visuelle permet aux marketeurs et aux concepteurs de créer et de modifier du contenu à l’aide d’une interface visuelle. De nombreux choix de conception peuvent être effectués sans avoir à modifier directement le code. L’édition de HTML et de JavaScript est également possible à l’aide des options d’édition disponibles dans le compositeur.

* Le contenu réside dans AEM et les éditeurs de contenu créent et gèrent les pages du site.
* Target utilise AEM pages hébergées du site pour exécuter des tests et de la personnalisation.
* Target diffuse du contenu personnalisé
* Le nouveau contenu réseau est créé à l’aide du VEC d’Adobe Target
* S’applique aux sites hébergés AEM et aux sites non AEM hébergés

   ![Personnalisation à l’aide du diagramme du compositeur d’expérience visuelle](assets/personalization-use-case-3/use-case-diagram-3.png)

**Pour mettre en oeuvre ce scénario, vous devez :**

* [Intégration d’AEM et d’Adobe Target à l’aide de Launch et d’Adobe I/O](./implementation.md#integrating-aem-target-options)

***Après la mise en oeuvre de l’intégration ci-dessus, nous vous proposons d’explorer la variable [en détail.](./personalization-use-case-3.md)***

## Personnalisation des expériences de pages Web complètes

L’intégration d’Adobe Experience Manager à Adobe Target vous permet de proposer une expérience personnalisée aux utilisateurs de votre site. En outre, il vous aide à mieux comprendre les versions du contenu de votre site web qui améliorent le mieux vos conversions au cours d’une période de test spécifiée. Par exemple, un test A/B compare plusieurs versions du contenu de votre site web afin d’identifier celle qui génère le plus de conversions, de ventes ou d’autres mesures que vous identifiez. Un spécialiste du marketing peut créer des activités dans Adobe Target afin de comprendre comment les utilisateurs interagissent avec le contenu de votre site et l’impact de celui-ci sur les mesures de votre site.

* Le contenu réside dans AEM et les éditeurs de contenu créent et gèrent les pages du site.
* Target utilise AEM pages hébergées du site pour exécuter des tests et de la personnalisation.
* Target diffuse du contenu personnalisé
* Aucun nouveau contenu net n’est créé ici
* S’applique aux sites AEM et non AEM

   ![diagramme](assets/personalization-use-case-2/use-case-2-diagram.png)

**Pour mettre en oeuvre ce scénario, vous devez :**

* [Intégration d’AEM et d’Adobe Target à l’aide de Launch et d’Adobe I/O](./implementation.md#integrating-aem-target-options)

***Après la mise en oeuvre de l’intégration ci-dessus, nous vous proposons d’explorer la variable [en détail.](./personalization-use-case-2.md)***
