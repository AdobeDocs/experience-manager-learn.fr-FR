---
title: Développement d’états de ressources dans AEM Sites
description: 'Les API Adobe Experience Manager Resource Status sont un framework enfichable permettant d’exposer les messages d’état dans AEM différentes interfaces utilisateur web de l’éditeur. '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 3%

---


# Développement des états de ressources {#developing-resource-statuses-in-aem-sites}

Les API Adobe Experience Manager Resource Status sont un framework enfichable permettant d’exposer les messages d’état dans AEM différentes interfaces utilisateur web de l’éditeur.

## Présentation {#overview}

La structure Resource Status for Editors fournit des API côté serveur et côté client pour l’affichage et l’interaction avec les états de l’éditeur d’une manière standard et uniforme.

Les barres d’état de l’éditeur sont nativement disponibles dans les éditeurs Page, Fragment d’expérience et Modèle d’AEM.

Voici des exemples de cas d’utilisation pour les fournisseurs d’état de ressource personnalisés :

* Notifier les auteurs lorsqu’une page est dans les 2 heures suivant l’activation planifiée
* Notifier les auteurs qu’une page a été activée au cours des 15 dernières minutes
* Notifier les auteurs qu’une page a été modifiée au cours des 5 dernières minutes et par qui

![Présentation de l’état des ressources de l’éditeur AEM](assets/sample-editor-resource-status-screenshot.png)

## Structure du fournisseur d’état des ressources {#resource-status-provider-framework}

Lors du développement d’états de ressources personnalisés, le travail de développement comprend :

1. Mise en oeuvre de ResourceStatusProvider, chargée de déterminer si un état est requis, et les informations de base sur l’état : titre, message, priorité, variante, icône et actions disponibles.
2. Éventuellement, le code JavaScript de l’IU Granite qui met en oeuvre la fonctionnalité de toutes les actions disponibles.

   ![architecture de statut des ressources](assets/sample-editor-resource-status-application-architecture.png)

3. La ressource d’état fournie dans les éditeurs de page, de fragment d’expérience et de modèle se voit attribuer un type via les ressources &quot;[!DNL statusType]&quot;.

   * Éditeur de page : `editor`
   * Éditeur de fragment d’expérience : `editor`
   * Éditeur de modèles: `template-editor`

4. La ressource d’état `statusType` est associé à enregistré `CompositeStatusType` Configuration OSGi `name` .

   Pour toutes les correspondances, la variable `CompositeStatusType's` Les types sont collectés et utilisés pour collecter les `ResourceStatusProvider` implémentations de ce type, via `ResourceStatusProvider.getType()`.

5. La correspondance `ResourceStatusProvider` est transmis au `resource` dans l’éditeur, et détermine si la variable `resource` a le statut à afficher. Si l’état est nécessaire, cette implémentation est responsable de la création de 0 ou de plusieurs `ResourceStatuses` pour renvoyer la valeur , chacune représentant un état à afficher.

   En règle générale, une `ResourceStatusProvider` renvoie 0 ou 1 `ResourceStatus` per `resource`.

6. ResourceStatus est une interface qui peut être implémentée par le client ou qui peut fournir des informations utiles. `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` peut être utilisé pour construire un état. Un état comprend :

   * Titre
   * Message
   * Icône
   * Variante
   * Priorité
   * Actions
   * Données

7. Facultatif, si `Actions` sont fournis pour la variable `ResourceStatus` , les clientlibs prises en charge sont nécessaires pour lier la fonctionnalité aux liens d’action dans la barre d’état.

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. Tout code JavaScript ou CSS compatible avec les actions doit être envoyé par proxy via les bibliothèques clientes respectives de chaque éditeur pour s’assurer que le code frontal est disponible dans l’éditeur.

   * Catégorie de l’éditeur de page : `cq.authoring.editor.sites.page`
   * Catégorie de l’éditeur de fragments d’expérience : `cq.authoring.editor.sites.page`
   * Catégorie de l’éditeur de modèles : `cq.authoring.editor.sites.template`

## Afficher le code {#view-the-code}

[Voir le code sur GitHub](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## Ressources supplémentaires {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
