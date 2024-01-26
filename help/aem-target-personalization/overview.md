---
title: Prise en main d’AEM et d’Adobe Target
description: Tutoriel complet décrivant comment créer et offrir des expériences personnalisées à l’aide d’Adobe Experience Manager et d’Adobe Target. Dans ce tutoriel, vous découvrirez également les différentes personnes impliquées dans le processus de bout en bout et comment elles collaborent entre elles.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 219
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 100%

---

# Intégrer AEM Sites et Adobe Target {#getting-started-with-aem-target}

AEM et Target sont deux solutions puissantes offrant des fonctionnalités qui peuvent sembler redondantes. Les clientes et clients ont parfois du mal à comprendre comment et quand utiliser ces produits conjointement pour offrir une expérience personnalisée. Pour offrir une expérience optimisée à chaque utilisateur final ou utilisatrice finale, les différentes équipes de votre organisation doivent travailler en étroite collaboration et définir les tâches de chacun et chacune.

Dans ce tutoriel, nous présentons trois scénarios différents pour AEM et Target, qui vous aideront à comprendre ce qui fonctionne le mieux pour votre organisation et comment les différentes équipes collaborent.

* Scénario 1 : personnalisation à l’aide des fragments d’expérience AEM
* Scénario 2 : personnalisation à l’aide du compositeur d’expérience visuelle
* Scénario 3 : personnalisation des expériences de pages web complètes

## Personnalisation à l’aide des fragments d’expérience AEM {#personalization-using-aem-experience-fragment}

Pour ce scénario, nous allons utiliser AEM et Target. De toute évidence, les deux produits ont leurs propres points forts. Lorsqu’il s’agit de fournir des expériences personnalisées aux utilisateurs et utilisatrices de votre site, vous avez besoin de **contenu personnalisé (contenu d’AEM)** et d’une **méthode intelligente (Target)** pour diffuser ces contenus en fonction d’un utilisateur ou d’une utilisatrice spécifique.

AEM vous aide à créer du contenu personnalisé en rassemblant l’ensemble de vos contenus et ressources à un emplacement central, afin d’alimenter votre stratégie de personnalisation. AEM permet de créer facilement du contenu à un seul endroit pour les ordinateurs de bureau, les tablettes et les appareils mobiles, sans devoir écrire de code. Il n’est pas nécessaire de créer des pages pour chaque appareil : AEM ajuste automatiquement chaque expérience à l’aide de votre contenu. Vous pouvez également exporter le contenu d’AEM vers Adobe Target sous forme d’offres en appuyant simplement sur un bouton.

Nous disposons désormais de contenu personnalisé sous la forme d’offres d’AEM dans Target. Target vous permet de diffuser ces offres à grande échelle sur la base d’une combinaison d’approches de machine learning basées sur des règles et pilotées par l’IA, qui intègrent des variables comportementales, contextuelles et hors ligne.  Avec Target, vous pouvez facilement configurer et exécuter des activités A/B et multivariées (MVT) afin de déterminer les offres, expériences et contenus les mieux adaptés.

Les **Fragments d’expérience** représentent un énorme pas en avant permettant de lier les créateurs et créatrices de contenu ou d’expérience aux professionnelles et professionnels de la personnalisation qui génèrent des résultats commerciaux à l’aide de Target.

* L’éditeur de contenu d’AEM crée du contenu personnalisé en tant que fragments d’expérience et leurs variantes.
* AEM exporte le HTML de Fragment d’expérience vers Target.
* Target utilise le balisage de Fragment d’expérience d’AEM comme offres dans les Activities.
* Target diffuse le HTML de Fragment d’expérience et AEM fournit des images référencées.

  ![Personnalisation à l’aide du diagramme de Fragments d’expérience.](assets/personalization-use-case-1/use-case-1-diagram.png)

**Pour implémenter ce scénario, vous devez effectuer les actions suivantes :**

* [Intégration d’AEM et d’Adobe Target à l’aide de Launch et d’Adobe I/O](./implementation.md#integrating-aem-target-options)
* [Intégration d’AEM et d’Adobe Target à l’aide de services cloud hérités](./implementation.md#integrating-aem-target-options)

***Après l’implémentation des intégrations ci-dessus, nous vous proposons d’explorer le [scénario en détail](./personalization-use-case-1.md).***

## Personnalisation à l’aide du compositeur d’expérience visuelle

Les professionnelles et professionnels du marketing peuvent apporter des modifications rapides à leur site web sans modifier de code pour exécuter un test à l’aide du compositeur d’expérience visuelle (VEC) d’Adobe Target. Le compositeur d’expérience visuelle est une interface utilisateur WYSIWYG (ce que vous voyez est ce que vous obtenez) qui vous permet de créer et de tester facilement des expériences et des offres personnalisées dans le contexte du site. Vous pouvez créer des expériences et des offres pour les Activites Target en effectuant un glisser-déposer, en permutant ou en modifiant la disposition et le contenu d’une page web (ou d’une offre) ou d’une page web mobile.

Le compositeur d’expérience visuelle est l’une des principales fonctionnalités d’Adobe Target. Le compositeur d’expérience visuelle permet aux professionnelles et professionnels du marketing et de la conception de créer et de modifier du contenu à l’aide d’une interface visuelle. Vous pouvez effectuer de nombreux choix sans avoir à modifier directement le code. La modification de HTML et de JavaScript est également possible à l’aide des options d’édition disponibles dans le compositeur.

* Le contenu se trouve dans AEM et les éditeurs et éditrices de contenu créent et gèrent les pages du site.
* Target utilise des pages hébergées par AEM pour exécuter des tests et personnaliser.
* Target diffuse du contenu personnalisé.
* Le nouveau contenu réseau est créé à l’aide du compositeur d’expérience visuelle d’Adobe Target.
* S’applique aux sites hébergés par AEM et aux autres sites.

  ![Personnalisation à l’aide du diagramme du compositeur d’expérience visuelle.](assets/personalization-use-case-3/use-case-diagram-3.png)

**Pour implémenter ce scénario, vous devez effectuer l’action suivante :**

* [Intégration d’AEM et d’Adobe Target à l’aide de Launch et d’Adobe I/O](./implementation.md#integrating-aem-target-options)

***Après l’implémentation de l’intégration ci-dessus, nous vous proposons d’explorer le [scénario en détail.](./personalization-use-case-3.md)***

## Personnalisation des expériences de pages web complètes

L’intégration d’Adobe Experience Manager à Adobe Target vous permet de proposer une expérience personnalisée aux utilisateurs et utilisatrices de votre site. En outre, elle vous aide à mieux comprendre les versions du contenu de votre site web qui améliorent le mieux vos conversions au cours d’une période de test spécifiée. Par exemple, un test A/B compare plusieurs versions du contenu de votre site web afin d’identifier celle qui génère le plus de conversions, de ventes ou d’autres mesures que vous identifiez. Un ou une spécialiste du marketing peut créer des Activities dans Adobe Target pour comprendre comment les utilisateurs et utilisatrices interagissent avec le contenu de votre site et comment cela affecte ses mesures.

* Le contenu se trouve dans AEM et les éditeurs et éditrices de contenu créent et gèrent les pages du site.
* Target utilise des pages hébergées par AEM pour exécuter des tests et personnaliser.
* Target diffuse du contenu personnalisé.
* Aucun nouveau contenu réseau n’est créé ici.
* S’applique aux sites AEM et aux autres sites.

  ![Diagramme.](assets/personalization-use-case-2/use-case-2-diagram.png)

**Pour implémenter ce scénario, vous devez effectuer l’action suivante :**

* [Intégration d’AEM et d’Adobe Target à l’aide de Launch et d’Adobe I/O](./implementation.md#integrating-aem-target-options)

***Après l’intégration ci-dessus, nous vous proposons d’explorer le [scénario en détail.](./personalization-use-case-2.md)***
