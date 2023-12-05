---
title: Développer des statuts de ressources dans AEM Sites
description: Les API de statut des ressources d’Adobe Experience Manager constituent un framework enfichable permettant d’afficher les messages de statut dans les différentes interfaces utilisateur web de l’éditeur d’AEM.
doc-type: Tutorial
version: 6.4, 6.5
duration: 133
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 100%

---


# Développer des statuts de ressources {#developing-resource-statuses-in-aem-sites}

Les API de statut des ressources d’Adobe Experience Manager constituent un framework enfichable permettant d’afficher les messages de statut dans les différentes interfaces utilisateur web de l’éditeur d’AEM.

## Vue d’ensemble {#overview}

Le framework du statut des ressources pour les éditeurs fournit des API côté serveur et côté client pour l’affichage et l’interaction avec les statuts de l’éditeur d’une manière standard et uniforme.

Les barres de statut de l’éditeur sont intégrées nativement aux éditeurs de pages, de fragments d’expérience et de modèles d’AEM.

Voici des exemples de cas d’utilisation pour les fournisseurs de statut de ressources personnalisés :

* Avertir les auteurs et autrices de toute activation planifiée d’une page dans les 2 heures.
* Avertir les auteurs at autrices de toute activation d’une page au cours des 15 dernières minutes.
* Avertir les auteurs at autrices de toute modification d’une page au cours des 5 dernières minutes et de la personne responsable.

![Vue d’ensemble du statut des ressources de l’éditeur AEM.](assets/sample-editor-resource-status-screenshot.png)

## Framework du fournisseur de statut des ressources {#resource-status-provider-framework}

Lors du développement de statuts de ressources personnalisés, le travail de développement comprend les éléments suivants :

1. La mise en œuvre de ResourceStatusProvider, qui permet de déterminer si un statut est requis, et les informations de base sur le statut : titre, message, priorité, variante, icône et actions disponibles.
2. De manière facultative, le code JavaScript de l’IU Granite qui met en œuvre la fonctionnalité des actions disponibles.

   ![Architecture de statut des ressources.](assets/sample-editor-resource-status-application-architecture.png)

3. La ressource de statut fournie dans les éditeurs de pages, de fragments d’expérience et de modèles se voit attribuer un type via la propriété « [!DNL statusType] » des ressources.

   * Éditeur de pages : `editor`
   * Éditeur de fragments d’expérience : `editor`
   * Éditeur de modèles : `template-editor`

4. La propriété `statusType` de la ressource de statut est associée à la propriété `name` enregistrée `CompositeStatusType` dans la configuration OSGi.

   Pour toutes les correspondances, les types `CompositeStatusType's` sont collectés et utilisés pour collecter les implémentations `ResourceStatusProvider` de ce type, via `ResourceStatusProvider.getType()`.

5. La propriété `ResourceStatusProvider` correspondante reçoit la `resource` dans l’éditeur et détermine si la `resource` dispose d’un statut à afficher. Si le statut est requis, la mise en œuvre va créer 0 ou plusieurs propriétés `ResourceStatuses` à renvoyer, chacune représentant un statut à afficher.

   En règle générale, la propriété `ResourceStatusProvider` renvoie 0 ou 1 `ResourceStatus` par `resource`.

6. ResourceStatus est une interface qui peut être implémentée par le client ou la cliente. `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` permet de créer un statut de manière pratique. Un statut se compose des éléments suivants :

   * Titre
   * Message
   * Icône
   * Variante
   * Priorité
   * Actions
   * Données

7. De manière facultative, si les `Actions` sont fournis pour l’objet `ResourceStatus`, les bibliothèques clientes sous-jacentes sont nécessaires pour rendre les liens d’action effectifs dans la barre de statut.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Tout code JavaScript ou CSS permettant de rendre les actions effectives doit être envoyé par proxy via les bibliothèques clientes respectives de chaque éditeur afin de s’assurer que le code front-end est disponible dans l’éditeur.

   * Catégorie de l’éditeur de page : `cq.authoring.editor.sites.page`
   * Catégorie de l’éditeur de fragments d’expérience : `cq.authoring.editor.sites.page`.
   * Catégorie de l’éditeur de modèles : `cq.authoring.editor.sites.template`.

## Afficher le code {#view-the-code}

[Consulter le code sur GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Ressources supplémentaires {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
