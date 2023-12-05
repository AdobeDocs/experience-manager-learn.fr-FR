---
title: Créer une configuration du service cloud de Launch dans AEM Sites
description: Découvrez comment créer une configuration du service cloud de Launch dans AEM. La configuration du service cloud de Launch peut ensuite être appliquée à un site existant et le chargement des bibliothèques de balises peut être consulté dans les environnements de création et de publication.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 146
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 100%

---

# Créer une configuration de service cloud Launch dans AEM {#create-launch-cloud-service}

>[!NOTE]
>
>Le processus de changement de nom d’Adobe Experience Platform Launch en tant qu’ensemble de technologies de collecte de données est en cours de mise en œuvre dans l’interface utilisateur, le contenu et la documentation du produit AEM. Par conséquent, le terme Launch est toujours utilisé ici.

Découvrez comment créer une configuration du service cloud de Launch dans Adobe Experience Manager. La configuration du service cloud de Launch d’AEM peut ensuite être appliquée à un site existant et le chargement des bibliothèques de balises peut être consulté dans les environnements de création et de publication.

## Créer un service cloud de Launch

Créez la configuration du service cloud de Launch en suivant les étapes ci-dessous.

1. Dans le menu **Outils**, sélectionnez **Services cloud** et cliquez sur **Configurations Adobe Launch**.

1. Sélectionnez le dossier de configuration de votre site ou sélectionnez **WKND Site** (si vous utilisez le projet de guide WKND), puis cliquez sur **Créer**.

1. Dans l’onglet _Général_, nommez votre configuration à l’aide du champ **Titre** et sélectionnez **Adobe Launch** dans le menu déroulant _Configuration Adobe IMS associée_. Sélectionnez ensuite le nom de votre société dans le menu déroulant _Société_, et sélectionnez la propriété créée précédemment dans le menu déroulant _Propriété_.

1. Dans l’onglet _Évaluation_ et _Production_, conservez les configurations par défaut. Toutefois, il est recommandé de vérifier et de modifier les configurations pour la configuration de production réelle, en particulier le bouton (bascule) _Chargement asynchrone de la bibliothèque_ suivant vos besoins de performances et d’optimisation. Notez également que la valeur _URI de bibliothèque_ est différente dans l’environnement d’évaluation et de production.

1. Enfin, cliquez sur **Créer** pour terminer les services cloud de Launch.

   ![Configuration des services cloud de Launch.](assets/launch-cloud-services-config.png)

## Appliquer le service cloud de Launch au site

Pour charger les propriétés des balises et ses bibliothèques sur le site AEM, la configuration du service cloud de Launch est appliquée au site. À l’étape précédente, la configuration du service cloud est créée sous le dossier du nom du site (WKND Site). Elle doit donc être appliquée automatiquement. Vérifiez-la.

1. Dans le menu **Navigation**, sélectionnez l’icône **Sites**.

1. Sélectionnez la page racine du site AEM, puis cliquez sur **Propriétés**. Ensuite, accédez à l’onglet **Avancé** sous **Configuration**, vérifiez que la valeur de configuration du cloud pointe vers le dossier spécifique `conf` de votre site.

   ![Application d’une configuration de services cloud au site.](assets/apply-cloud-services-config-to-site.png)

## Vérifier le chargement de la propriété de balise sur les pages de création et de publication

Il est maintenant temps de vérifier que la propriété de la balise et ses bibliothèques sont chargées sur la page du site AEM.

1. Ouvrez votre page de site préférée en mode **Afficher comme publié**. Dans la console du navigateur, le message du journal doit s’afficher. Il s’agit du même message provenant de l’extrait de code JavaScript de la règle de propriété de la balise qui est déclenché lorsque l’événement _Bibliothèque chargée (Haut de page)_ est déclenché.

1. Pour vérifier la publication, commencez par publier votre configuration du **service cloud de Launch** et ouvrez la page du site sur l’instance de publication.

   ![Propriété de la balise sur les pages de création et de publication.](assets/tag-property-on-author-publish-pages.png)

Félicitations. Vous avez terminé l’intégration AEM et l’intégration de la balise de collecte de données qui injecte du code JavaScript dans votre site AEM sans mettre à jour le code de projet d’AEM.

## Défi : mettre à jour et publier la règle dans la propriété de la balise

Appuyez-vous sur les connaissances acquises lors de l’étape de [Création d’une propriété de balise](./create-tag-property.md) pour relever un défi simple : mettez à jour la règle existante afin d’ajouter une instruction de console supplémentaire et déployez-la sur le site AEM en utilisant la fonction _Flux de publication_.

## Étapes suivantes

[Déboguer une implémentation de balises](debug-tags-implementation.md)
