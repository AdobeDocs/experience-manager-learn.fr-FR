---
title: Visionneuses d’images, d’échantillons, à 360° et de supports variés
description: L’une des fonctionnalités les plus utiles et les plus puissantes de Dynamic Media Classic est sa prise en charge de la création des visionneuses de médias riches comme des visionneuses d’images, d’échantillons, à 360°, et de supports variés. Découvrez ce qu’est chaque visionneuse de médias riches et comment créer chaque type dans Dynamic Media Classic. Découvrez ensuite les paramètres prédéfinis de visionneuse par lot, qui automatisent le processus de création de visionneuses de médias riches lors du chargement.
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 307
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1359'
ht-degree: 100%

---

# Visionneuses d’images, d’échantillons, à 360° et de supports variés {#media-sets}

Au-delà des images uniques pour le dimensionnement et le zoom dynamiques, les collections de visionneuses Dynamic Media Classic offrent une expérience en ligne plus riche. Cette section du tutoriel explique comment créer les visionneuses de médias riches suivantes dans Dynamic Media Classic :

- Visionneuse d’images
- Visionneuse d’échantillons
- Visionneuse à 360°
- Visionneuse de supports variés

Elle explique également comment utiliser les paramètres prédéfinis de visionneuse par lot pour automatiser la création de visionneuses par le biais d’un chargement.

## Tout ce que vous souhaitez savoir sur les visionneuses.

Après le dimensionnement et le zoom dynamiques de base, les visionneuses sont probablement le sous-produit Dynamic Media Classic le plus utilisé. Les visionneuses sont essentiellement des ressources « virtuelles » qui ne contiennent pas d’images réelles, mais se composent d’un ensemble de relations avec d’autres images et/ou vidéos. L’attrait principal des visionneuses est qu’il s’agit de mini-applications prêtes à l’emploi. Cela signifie que chaque visionneuse d’ensemble contient sa propre logique et son interface, de sorte que tout ce que vous avez à faire est de l’appeler sur le site. En outre, il vous suffit de suivre un seul ID de ressource par visionneuse, plutôt que d’avoir à gérer toutes les ressources et relations membres vous-même.

Lorsque vous créez un ensemble, celui-ci est géré en tant que ressource distincte qui doit être marquée pour publication et publiée avant de pouvoir être diffusée à partir d’une URL. Toutes les ressources membres doivent également être publiées.

### Types de visionneuses

Découvrez les quatre types de visionneuses que vous pouvez créer dans Dynamic Media Classic : visionneuses d’image, d’échantillons, à 360° et de supports variés.

## Visionneuse d’images

Il s’agit du type de visionneuse le plus courant. Vous l’utiliserez généralement pour d’autres affichages du même élément. Elle se compose de plusieurs images que vous chargez dans la visionneuse en cliquant sur la miniature associée de cette image.

![image](assets/media-sets/image-set-1.jpg)

_Exemple de visionneuse d’images_

L’URL de la visionneuse d’images ci-dessus peut apparaître comme suit :

![image](assets/media-sets/image-set-url-1.png)

- Découvrez les visionneuses d’images avec le [Démarrage rapide pour les visionneuses d’images](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=fr).
- Découvrez comment [créer une visionneuse d’images](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=fr#creating-an-image-set).

### Visionneuse d’échantillons

Ce type de visionneuse est généralement utilisé pour afficher les vues colorées d’un même élément. Elle se compose de paires d’images et d’échantillons de couleurs.

La principale différence entre une visionneuse d’images et d’échantillons réside dans le fait que les visionneuses d’échantillons utilisent une image différente comme échantillon cliquable, tandis que les visionneuses d’images utilisent une version miniature de l’image d’origine, sous forme de miniature cliquable.

Les visionneuses d’échantillons ne colorent pas les images (il s’agit d’une idée fausse courante). Les images sont simplement échangées, exactement comme dans une visionneuse d’images. Les miniatures d’images d’échantillon auraient pu être créés à l’aide de Photoshop, chaque couleur aurait pu être photographiée séparément, ou l’outil Recadrage de Dynamic Media Classic aurait pu être utilisé pour créer un échantillon à partir d’une des images colorées.

![image](assets/media-sets/image-set-2.jpg)

_Exemple de visionneuse d’échantillons_

L’URL de la visionneuse d’échantillons ci-dessus peut se présenter comme suit :

![image](assets/media-sets/image-set_url.png)

- En savoir plus sur les visionneuses d’échantillons dans la section [Démarrage rapide pour les visionneuses d’échantillons](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=fr).
- Découvrez comment [Créer une visionneuse d’échantillons](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=fr#creating-a-swatch-set).

### Visionneuse à 360°

Cette visionneuse est généralement utilisée pour afficher une vue à 360 degrés d’un élément. À l’instar des visionneuses d’échantillons, les visionneuses à 360° ne créent pas de la 3D comme par magie, la dure réalité est de créer de nombreuses photos d’une image sous tous les angles. La visionneuse vous permet simplement de passer d’une image à l’autre, comme dans une animation image par image.

Les visionneuses à 360° peuvent effectuer une rotation dans une seule direction et le long d’un axe unique. À l’inverse, les visionneuses à 360° en 2D peuvent effectuer une rotation sur plusieurs axes. Par exemple, une voiture peut être pivotée pendant que toutes les roues sont au sol, puis peut être « retournée » et faire également l’objet d’une rotation sur ses roues arrière. Pour une visionneuse à 360° en 2D correctement configurée, le nombre d’images par ligne pour chaque axe doit être le même. En d’autres termes, si vous effectuez une rotation sur deux axes, vous avez besoin de deux fois plus d’images qu’avec un seul angle de rotation.

![image](assets/media-sets/image-set-3.png)

_Exemple de visionneuse à 360°_

L’URL de la visionneuse à 360° ci-dessus peut se présenter comme suit :

![image](assets/media-sets/spin-set.png)

- En savoir plus sur les visionneuses à 360° dans la section [Démarrage rapide pour les visionneuses à 360°](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=fr).
- Découvrez comment [Créer une visionneuse à 360°](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=fr#creating-a-spin-set).

## Visionneuse de supports variés

Il s’agit d’une visionneuse combinée. Cette visionneuse vous permet de combiner n’importe laquelle des visionneuses précédentes, ainsi que d’ajouter une vidéo, dans une seule visionneuse. Dans ce workflow, vous créez d’abord chacun des composants de la visionneuses, puis les assemblez dans une visionneuse de supports variés.

![image](assets/media-sets/image-set-4.png)

_Exemple de visionneuse de supports variés_

L’URL de la visionneuse de supports variés ci-dessus peut se présenter comme suit :

![image](assets/media-sets/image-set-url-1.png)

- En savoir plus sur les visionneuses de supports variés dans la section [Démarrage rapide pour les visionneuses de supports variés](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=fr).

- Découvrez comment [Créer une visionneuse de supports variés](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=fr#creating-a-mixed-media-set).

Pour afficher une image à zoomer, une visionneuse ou une vidéo sur votre site web, vous devez l’appeler dans une « visionneuse » Dynamic Media Classic. Dynamic Media Classic comprend des visionneuses pour les ressources de médias riches, telles que les visionneuses d’échantillons, les visionneuses à 360°, la vidéo, etc.

En savoir plus sur les [Visionneuses pour AEM Assets et Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=fr).

## Paramètres prédéfinis d’ensemble par lot

Jusqu’à présent, nous avons vu la création manuelle de visionneuses à l’aide de la fonction de création de Dynamic Media Classic. Cependant, il est possible d’automatiser la création de visionneuses d’images et de visionneuses à 360° à l’aide d’un paramètre prédéfini de visionneuse par lot, tant que vous disposez d’une convention de nommage normalisée.

Chaque paramètre prédéfini est un ensemble d’instructions indépendant possédant un nom unique. Il définit comment construire la visionneuse à l’aide d’images correspondant aux conventions de nommage définies. Dans le paramètre prédéfini, vous définissez d’abord des conventions de nommage pour les ressources que vous souhaitez regrouper dans un ensemble. Un paramètre prédéfini de visionneuse par lot peut ensuite être créé pour référencer ces images.

Bien qu’il soit possible de créer vous-même le paramètre prédéfini (sous **Configuration > Configuration de l’application > Paramètres prédéfinis de visionneuse par lot**), il est recommandé de confier cette tâche à votre équipe de conseil ou au support technique. La raison est la suivante :

- Les paramètres prédéfinis de visionneuse par lot peuvent être complexes à configurer. Ils sont alimentés par des expressions régulières. Cette syntaxe peut sembler inhabituelle voire déroutante, sauf si vous êtes un développeur ou une développeuse.
- Une fois créés, ils sont activés par défaut. Il n’existe pas de fonction « Annuler ». Si vous commencez à charger des milliers d’images et que votre paramètre prédéfini n’est pas correctement configuré, vous risquez de rencontrer des centaines ou des milliers de visionneuses rompues que vous devez rechercher et supprimer manuellement.

Une simple convention de nommage a été proposée plus tôt, qui serait très facile à intégrer dans un paramètre prédéfini de visionneuse par lot. Cependant, comme les paramètres prédéfinis sont très flexibles, ils peuvent gérer des stratégies de nommages élaborées. En sommes, les images qui font partie d’un ensemble doivent être associées sous un nom commun. Il s’agit souvent du numéro de SKU ou de l’ID de produit. Dans Dynamic Media Classic, vous pouvez soit donner une convention de nommage par défaut pour toutes vos images à utiliser pour un paramètre prédéfini, soit créer plusieurs paramètres prédéfinis, chacun avec des règles de nommage différentes.

Les paramètres prédéfinis de visionneuse par lot ne sont appliqués qu’au chargement. Ils perdent toute utilité une fois les images chargées. Il est donc important de planifier votre convention de nommage et de créer un paramètre prédéfini avant de commencer à charger toutes vos images.

Une fois les paramètres prédéfinis créés, l’administration de l’entreprise peut choisir s’ils sont actifs ou inactifs. Actif signifie qu’ils s’afficheront sur la page de chargement sous **Options de traitement**, tandis que les paramètres prédéfinis inactifs resteront masqués.

Découvrez comment [Créer un paramètre prédéfini de visionneuse par lot](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=fr#creating-a-batch-set-preset).

### Utiliser les paramètres prédéfinis de visionneuse par lot lors du chargement

Voici comment utiliser les paramètres prédéfinis de visionneuse par lot lors du chargement après leur création :

1. Cliquez sur **Charger** et choisissez **À partir du bureau** ou **Via FTP**.
2. Cliquez sur **Options de traitement**.
3. Ouvrez l’option **Paramètres prédéfinis de visionneuse par lot** et cochez ou désélectionnez le paramètre prédéfini à utiliser lors du chargement.
4. Une fois le chargement terminé, recherchez dans votre dossier les visionneuses terminées.

En savoir plus sur les [Paramètres prédéfinis de visionneuse par lot](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=fr#batch-set-presets).
