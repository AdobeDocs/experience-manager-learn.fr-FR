---
title: Points d’arrêt réactifs
description: Découvrez comment configurer de nouveaux points d’arrêt réactifs pour AEM éditeur de page réactif.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
source-git-commit: c82965636ddeef7dc165e0bea079c99f1a16e0ca
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---


# Points d’arrêt réactifs

Découvrez comment configurer de nouveaux points d’arrêt réactifs pour AEM éditeur de page réactif.

## Création de points d’arrêt CSS

Tout d’abord, créez des points d’arrêt multimédia dans le fichier CSS à grille réactive AEM auquel le site AEM réactif adhère.

Dans `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less` , créez les points d’arrêt à utiliser avec l’émulateur mobile. Prenez note de la `max-width` pour chaque point d’arrêt, car cela mappe les points d’arrêt CSS aux points d’arrêt de l’éditeur de page réactif AEM.

![Création de points d’arrêt réactifs](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personnalisation des points d’arrêt du modèle

Ouvrez le `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` fichier et mise à jour `cq:responsive/breakpoints` avec vos nouvelles définitions de noeud de point d’arrêt. Chaque [Point d’arrêt CSS](#create-new-css-breakpoints) doit avoir un noeud correspondant sous `breakpoints` avec `width` définie sur le point d’arrêt CSS `max-width`.

![Personnalisation des points d’arrêt réactifs du modèle](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Création d’émulateurs

AEM émulateurs doivent être définis pour permettre aux auteurs de sélectionner la vue réactive à modifier dans l’éditeur de page.

Créez des noeuds d’émulateurs sous `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Par exemple, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copie d’un noeud d’émulateur de référence à partir de `/libs/wcm/mobile/components/emulators` dans CRXDE Lite et mettez à jour la copie pour accélérer la définition du noeud.

![Création d’émulateurs](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Création d’un groupe d’appareils

Regroupez les émulateurs vers [les rendre disponibles dans AEM éditeur de page](#update-the-templates-device-group).

Créer `/apps/settings/mobile/groups/<name of device group>` structure de noeud sous `/ui.apps/src/main/content/jcr_root`.

![Création d’un groupe d’appareils](./assets/responsive-breakpoints/create-new-device-group.jpg)

Créez un `.content.xml` dans `/apps/settings/mobile/groups/<device group name>` et définissez les nouveaux émulateurs à l’aide d’un code similaire à celui ci-dessous :

![Création d’un périphérique](./assets/responsive-breakpoints/create-new-device.jpg)

## Mettre à jour le groupe d’appareils du modèle

Enfin, réassociez le groupe d’appareils au modèle de page afin que les émulateurs soient disponibles dans l’éditeur de page pour les pages créées à partir de ce modèle.

Ouvrez le `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` et mettre à jour le fichier `cq:deviceGroups` pour référencer le nouveau groupe mobile (par exemple, `cq:deviceGroups="[mobile/groups/customdevices]"`)
