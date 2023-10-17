---
title: Points d’arrêt réactifs
description: Découvrez comment configurer de nouveaux points d’arrêt réactifs pour l’éditeur de page réactif AEM.
version: Cloud Service
feature: Page Editor
topic: Mobile, Development
role: Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-01-05T00:00:00Z
kt: 11664
thumbnail: kt-11664.jpeg
exl-id: 8b48c28f-ba7f-4255-be96-a7ce18ca208b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '256'
ht-degree: 100%

---

# Points d’arrêt réactifs

Découvrez comment configurer de nouveaux points d’arrêt réactifs pour l’éditeur de page réactif AEM.

## Créer des points d’arrêt CSS

Tout d’abord, créez des points d’arrêt multimédia dans le fichier CSS à grille réactive AEM auquel le site AEM réactif adhère.

Dans le fichier `/ui.apps/src/main/content/jcr_root/apps/[app name]/clientlibs/clientlib-grid/less/grid.less`, créez les points d’arrêt à utiliser avec l’émulateur mobile. Prenez note de la `max-width` pour chaque point d’arrêt, car cela mappe les points d’arrêt CSS aux points d’arrêt de l’éditeur de page réactif AEM.

![Création de points d’arrêt réactifs.](./assets/responsive-breakpoints/create-new-breakpoints.jpg)

## Personnaliser les points d’arrêt du modèle

Ouvrez le fichier `ui.content/src/main/content/jcr_root/conf/<app name>/settings/wcm/templates/page-content/structure/.content.xml` et mettez à jour `cq:responsive/breakpoints` avec vos nouvelles définitions de nœud de point d’arrêt. Chaque [point d’arrêt CSS](#create-new-css-breakpoints) doit avoir un nœud correspondant sous `breakpoints` avec sa propriété `width` définie sur la `max-width` du point d’arrêt CSS.

![Personnalisation des points d’arrêt réactifs du modèle.](./assets/responsive-breakpoints/customize-template-breakpoints.jpg)

## Créer des émulateurs

Les émulateurs AEM doivent être définis pour permettre aux auteurs et aux autrices de sélectionner la vue réactive à modifier dans l’éditeur de page.

Créer des nœuds d’émulateurs sous `/ui.apps/src/main/content/jcr_root/apps/<app name>/emulators`

Par exemple, `/ui.apps/src/main/content/jcr_root/apps/wknd-examples/emulators/phone-landscape`. Copiez un nœud d’émulateur de référence à partir de `/libs/wcm/mobile/components/emulators` dans CRXDE Lite et mettez à jour la copie pour accélérer la définition du nœud.

![Création d’émulateurs.](./assets/responsive-breakpoints/create-new-emulators.jpg)

## Créer un groupe d’appareils

Regroupez les émulateurs afin de [les rendre disponibles dans l’éditeur de page AEM](#update-the-templates-device-group).

Créez la structure de nœud `/apps/settings/mobile/groups/<name of device group>` sous `/ui.apps/src/main/content/jcr_root`.

![Création d’un groupe d’appareils.](./assets/responsive-breakpoints/create-new-device-group.jpg)

Créez un fichier `.content.xml` dans `/apps/settings/mobile/groups/<device group name>` et définissez les nouveaux émulateurs à l’aide d’un code similaire à celui ci-dessous :

![Création d’un appareil.](./assets/responsive-breakpoints/create-new-device.jpg)

## Mettre à jour le groupe d’appareils du modèle

Enfin, réassociez le groupe d’appareils au modèle de page afin que les émulateurs soient disponibles dans l’éditeur de page pour les pages créées à partir de ce modèle.

Ouvrez le fichier `ui.content/src/main/content/jcr_root/conf/[app name]/settings/wcm/templates/page-content/structure/.content.xml` et mettez à jour la propriété `cq:deviceGroups` pour référencer le nouveau groupe mobile (par exemple, `cq:deviceGroups="[mobile/groups/customdevices]"`).
