---
title: Visionneuses d’images, d’échantillons, à 360° et de supports variés
description: L’une des fonctionnalités les plus utiles et les plus puissantes de Dynamic Media Classic est sa prise en charge de la création de visionneuses de médias enrichis, telles que les visionneuses d’images, d’échantillons, à 360° et de supports variés. Découvrez ce qu’est chaque visionneuse de médias enrichis et comment créer chaque type dans Dynamic Media Classic. En savoir plus sur les paramètres prédéfinis d’ensemble par lot, qui automatisent le processus de création de visionneuses de médias riches lors du téléchargement.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 2%

---

# Visionneuses d’images, d’échantillons, à 360° et de supports variés {#media-sets}

Au-delà des images uniques pour le dimensionnement et le zoom dynamiques, les collections de visionneuses Dynamic Media Classic offrent une expérience en ligne plus riche. Cette section du tutoriel explique comment créer les visionneuses de médias enrichis suivantes dans Dynamic Media Classic :

- Visionneuse d’images
- Série d’échantillons
- Visionneuse à 360°
- Visionneuse de médias mixtes

Il explique également comment utiliser les paramètres prédéfinis d’ensemble par lot pour automatiser la création d’un ensemble par le biais d’un chargement.

## Tout ce que vous souhaitez savoir sur les visionneuses

En plus du dimensionnement dynamique de base et du zoom, les visionneuses sont probablement le sous-produit Dynamic Media Classic le plus utilisé. Les visionneuses sont essentiellement des ressources &quot;virtuelles&quot; qui ne contiennent pas d’images réelles, mais se composent d’un ensemble de relations avec d’autres images et/ou vidéos. L&#39;attrait principal des ensembles est qu&#39;il s&#39;agit de mini-applications prêtes &quot;à l&#39;emploi&quot;. Cela signifie que chaque visionneuse d’ensemble contient sa propre logique et son interface, de sorte que tout ce que vous avez à faire est de l’appeler sur le site. En outre, il vous suffit de suivre un seul ID de ressource par ensemble, plutôt que d’avoir à gérer toutes les ressources et relations membres vous-même.

Lorsque vous créez un jeu, celui-ci est géré en tant que ressource distincte qui doit être marquée pour publication et publiée avant de pouvoir être diffusée à partir d’une URL. Toutes les ressources membres doivent également être publiées.

### Types d’ensembles

Découvrez les quatre types d’ensembles que vous pouvez créer dans Dynamic Media Classic : Visionneuses d’images, d’échantillons, à 360° et de supports variés.

## Visionneuse d’images

Il s’agit du type d’ensemble le plus courant. Vous l’utiliserez généralement pour d’autres affichages du même article. Il se compose de plusieurs images que vous chargez dans la visionneuse en cliquant sur la miniature associée de cette image.

![image](assets/media-sets/image-set-1.jpg)

_Exemple de visionneuse d’images_

L’URL de la visionneuse d’images ci-dessus peut apparaître comme suit :

![image](assets/media-sets/image-set-url-1.png)

- En savoir plus sur les visionneuses d’images avec [Démarrage rapide pour les visionneuses d’images](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- Découvrez comment [Création d’une visionneuse d’images](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### Série d’échantillons

Ce type d’ensemble est généralement utilisé pour afficher les vues en couleur d’un même élément. Il se compose de paires d’images et d’échantillons de couleurs.

La principale différence entre une visionneuse d’images et d’échantillons réside dans le fait que les visionneuses d’échantillons utilisent une image différente comme échantillon cliquable, tandis que les visionneuses d’images utilisent une version miniature de l’image d’origine, sous forme de miniature cliquable.

Les séries d’échantillons ne colorent pas les images (une idée fausse courante). Les images sont simplement échangées, exactement comme dans une visionneuse d’images. Les mini-échantillons d’images auraient pu être créés à l’aide de Photoshop, chaque couleur aurait pu être photographiée séparément ou l’outil Recadrer de Dynamic Media Classic aurait pu être utilisé pour créer un échantillon à partir d’une des images colorées.

![image](assets/media-sets/image-set-2.jpg)

_Exemple d’une série d’échantillons_

L’URL de la série d’échantillons ci-dessus peut apparaître comme suit :

![image](assets/media-sets/image-set_url.png)

- En savoir plus sur les séries d’échantillons avec [Démarrage rapide des séries d’échantillons](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- Découvrez comment [Création d’une série d’échantillons](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### Visionneuse à 360°

Cet ensemble est généralement utilisé pour afficher une vue à 360 degrés d’un élément. Comme les séries d’échantillons, les visionneuses à 360° n’utilisent pas de magie 3D — le vrai travail est de créer de nombreuses photos d’une image de tous les côtés. La visionneuse vous permet simplement de basculer entre les images comme une animation stop.

Les visionneuses à 360° peuvent tourner dans une direction le long d’un axe unique, ou si elles sont créées à l’inverse sous la forme d’une visionneuse à 360° en 2D (rotation sur plusieurs axes). Par exemple, une voiture peut être pivotée pendant que toutes les roues sont au sol, puis peut être &quot;retournée&quot; et faire également l’objet d’une rotation sur ses roues arrière. Pour une visionneuse à 360° en 2D correctement configurée, le nombre d’images par ligne pour chaque axe doit être le même. En d’autres termes, si vous tournez sur deux axes, vous avez besoin de deux fois plus d’images qu’un seul angle de rotation.

![image](assets/media-sets/image-set-3.png)

_Exemple de visionneuse à 360°_

L’URL de la visionneuse à 360° ci-dessus peut apparaître comme suit :

![image](assets/media-sets/spin-set.png)

- En savoir plus sur les visionneuses à 360° avec la méthode [Démarrage rapide pour les visionneuses à 360°](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- Découvrez comment [Création d’une visionneuse à 360°](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## Visionneuse de médias mixtes

Il s’agit d’un ensemble de combinaisons. Il vous permet de combiner n’importe lequel des visionneuses précédentes, ainsi que d’ajouter une vidéo, dans une seule visionneuse. Dans ce workflow, vous créez d’abord l’un des ensembles de composants, puis vous les assemblez dans une visionneuse de supports variés.

![image](assets/media-sets/image-set-4.png)

_Exemple de visionneuse de médias mixtes_

L’URL de la visionneuse de médias mixtes ci-dessus peut apparaître comme suit :

![image](assets/media-sets/image-set-url-1.png)

- En savoir plus sur les visionneuses de médias mixtes avec [Démarrage rapide pour les visionneuses de médias mixtes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- Découvrez comment [Création d’une visionneuse de médias mixtes](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

Pour afficher une image de zoom, une visionneuse ou une vidéo sur votre site web, vous l’appelez dans une &quot;visionneuse&quot; Dynamic Media Classic. Dynamic Media Classic comprend des visionneuses pour les ressources multimédias enrichies telles que les séries d’échantillons, les visionneuses à 360°, la vidéo, etc.

En savoir plus sur [Visionneuses pour AEM Assets et Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=fr).

## Paramètres prédéfinis de lot

Jusqu’à présent, nous avons discuté de la création manuelle de visionneuses à l’aide de la fonction de création Dynamic Media Classic. Cependant, il est possible d’automatiser la création de visionneuses d’images et de visionneuses à 360° à l’aide d’un paramètre prédéfini d’ensemble par lot tant que vous disposez d’une convention d’affectation de nom normalisée.

Chaque paramètre prédéfini est un ensemble d’instructions indépendant à nom unique qui définit la manière de créer la visionneuse à l’aide d’images qui correspondent aux conventions d’affectation de nom définies. Dans le paramètre prédéfini, vous définissez d’abord des conventions d’affectation de nom pour les ressources que vous souhaitez regrouper dans un ensemble. Un paramètre prédéfini d’ensemble par lot peut ensuite être créé pour référencer ces images.

Bien qu’il soit possible de créer vous-même le paramètre prédéfini (il se trouve sous **Configuration > Configuration de l’application > Paramètres prédéfinis d’ensemble par lot** ), il est recommandé que votre équipe de conseillers ou votre support technique le configurent pour vous. Voici pourquoi :

- Les paramètres prédéfinis d’ensemble par lot peuvent être complexes à configurer. Ils sont alimentés par des expressions régulières. Cette syntaxe peut être peu familière ou déroutante, sauf si vous êtes un développeur.
- Une fois créés, ils sont activés par défaut. Il n’existe pas de fonction &quot;Annuler&quot;. Si vous commencez à charger des milliers d’images et que votre paramètre prédéfini n’est pas correctement configuré, vous risquez de rencontrer des centaines ou des milliers d’ensembles rompus que vous devez rechercher et supprimer manuellement.

Une simple convention d’affectation des noms a été proposée plus tôt, qui serait très facile à intégrer dans un paramètre prédéfini d’ensemble par lot. Cependant, comme les paramètres prédéfinis sont très flexibles, ils peuvent gérer des stratégies de dénomination complexes. En bref, les images qui font partie d’un ensemble doivent être liées par un nom commun, souvent c’est le numéro de SKU ou l’ID de produit. Dans Dynamic Media Classic, vous pouvez soit lui donner une convention d’affectation de nom par défaut pour toutes vos images à utiliser pour un paramètre prédéfini, soit créer plusieurs paramètres prédéfinis, chacun avec des règles d’affectation de nom différentes.

Les paramètres prédéfinis d’ensemble par lot ne sont appliqués qu’au chargement. elles ne peuvent pas être exécutées une fois les images téléchargées. Il est donc important de planifier votre convention d’affectation des noms et de créer un paramètre prédéfini avant de commencer à charger toutes vos images.

Une fois les paramètres prédéfinis créés, l’administrateur de l’entreprise peut choisir s’ils sont principaux ou inactifs. Principal signifie qu’ils s’afficheront sur la page de chargement sous **Options de tâche**, tandis que les paramètres prédéfinis inactifs restent masqués.

Découvrez comment [Création d’un paramètre prédéfini d’ensemble par lot](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### Utilisation de paramètres prédéfinis d’ensemble par lot lors du téléchargement

Voici comment vous utilisez les paramètres prédéfinis d’ensemble par lot lors du chargement après leur création :

1. Cliquez sur **Télécharger** et choisissez **À partir du bureau** ou **Via FTP**.
2. Cliquez sur **Options de tâche**.
3. Ouvrez le **Paramètres prédéfinis d’ensemble par lot** et cochez ou désélectionnez le paramètre prédéfini à utiliser avec le téléchargement.
4. Une fois le transfert terminé, recherchez dans votre dossier les visionneuses terminées.

En savoir plus sur [Paramètres prédéfinis d’ensemble par lot](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
