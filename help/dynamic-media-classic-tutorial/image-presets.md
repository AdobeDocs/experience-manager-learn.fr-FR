---
title: Paramètres d’image prédéfinis
description: Paramètres d’image prédéfinis Dans Dynamic Media Classic, tous les paramètres nécessaires pour créer une image à une taille, un format, une qualité et une accentuation spécifiques sont inclus. Les paramètres d’image prédéfinis sont un composant clé du dimensionnement dynamique. Lorsque vous examinez une URL dans Dynamic Media Classic, vous pouvez facilement voir si un paramètre d’image prédéfini est en cours d’utilisation. Découvrez les paramètres d’image prédéfinis, pourquoi ils sont si utiles et comment en créer un.
sub-product: dynamic-media
feature: Dynamic Media Classic, Image Presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---


# Paramètres d’image prédéfinis {#image-presets}

Un paramètre d’image prédéfini est essentiellement une recette qui contient tous les paramètres nécessaires pour créer une image à une taille, un format, une qualité et une accentuation spécifiques. Les paramètres d’image prédéfinis sont un composant clé du dimensionnement dynamique.

Si vous examinez les URL d’à peu près n’importe quel client Dynamic Media Classic, vous verrez probablement un paramètre d’image prédéfini en cours d’utilisation. Il vous suffit de rechercher $name$ à la fin de l’URL (avec un ou plusieurs mots remplacés par le nom).

Les paramètres d’image prédéfinis raccourcissent l’URL. Ainsi, au lieu d’écrire plusieurs instructions de diffusion d’images par requête, vous pouvez écrire un seul paramètre d’image prédéfini. Par exemple, ces deux URL produisent la même image JPEG 300 x 300 avec accentuation, mais la seconde utilise un paramètre d’image prédéfini :

![image](assets/image-presets/image-preset-2.png)

La valeur réelle des paramètres d’image prédéfinis est que tout administrateur d’entreprise peut mettre à jour la définition de ce paramètre d’image prédéfini et affecter chaque image utilisant ce format, sans modifier de code web. Vous verrez les résultats de toute modification apportée à un paramètre d’image prédéfini après l’effacement du cache de l’URL.

>[!IMPORTANT]
>
>Lors du redimensionnement d’une image, le rapport L/H, le rapport largeur/hauteur de l’image, doit toujours être conservé proportionnel afin que l’image ne soit pas déformée.

Un paramètre d’image prédéfini comporte un symbole dollar ($) des deux côtés de son nom et suit le point d’interrogation (?) séparateur.

>[!TIP]
>
>Créez un paramètre d’image prédéfini par taille d’image unique sur votre site. Par exemple, si vous avez besoin d’une image 350 x 350 pour la page des détails du produit, d’une image 120 x 120 pour les pages de navigation/recherche et d’une image 90 x 90 pour une vente croisée/un article présenté, vous avez besoin de trois paramètres d’image prédéfinis, que vous ayez 500 00.

- En savoir plus sur les [paramètres d’image prédéfinis](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Découvrez comment [créer un paramètre d’image prédéfini](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Paramètres d’image prédéfinis et accentuation

Les paramètres d’image prédéfinis redimensionnent généralement une image. Dès que vous redimensionnez une image à partir de sa taille d’origine, vous devez ajouter une accentuation. Cela est dû au fait que le redimensionnement entraîne la fusion et la fusion de nombreux pixels dans un espace plus petit, ce qui rend l’image douce et floue. L’accentuation augmente le contraste des bords et des zones à fort contraste dans une image.

Nous prévoyons que les images haute résolution que vous téléchargez dans Dynamic Media Classic n’ont pas besoin d’accentuation lorsqu’elles sont affichées en taille réelle, lors d’un zoom avant. Toutefois, à une taille plus petite, une accentuation est généralement souhaitable.

>[!TIP]
>
>Toujours accentuer lors du redimensionnement des images Cela signifie que vous devrez ajouter une accentuation à chaque paramètre d’image prédéfini (et au paramètre prédéfini de visionneuse, dont nous parlerons plus loin).
>
>Si vos images n’ont pas l’air en forme, c’est probablement parce qu’elles ont besoin d’une accentuation ou peut-être que la qualité était médiocre pour commencer.

Le degré d’accentuation à ajouter est entièrement subjectif. Certaines personnes aiment les images plus douces, tandis que d&#39;autres les apprécient très nettement. Il est facile d’améliorer une image en exécutant une combinaison de filtres d’accentuation sur une image. Cependant, il est également facile de surcharger et d’accentuer une image de manière excessive.

Le graphique suivant montre trois niveaux d’accentuation. De droite à gauche vous n&#39;avez pas d&#39;accentuation, juste la bonne quantité, et trop.

![image](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic permet trois types d’accentuation : Accentuation simple, mode Rééchantillonnage et masquage flou.

En savoir plus sur les [options d’accentuation de Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Ressources supplémentaires

[Guide des paramètres d’image prédéfinis](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Paramètres à utiliser pour optimiser la qualité de l’image et la vitesse de chargement.

[L&#39;Image C&#39;Est Tout, Partie 2 : Ce n&#39;est jamais juste un flou — Qualité contre vitesse](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Article de blog traitant de l’utilisation des paramètres d’image prédéfinis pour la diffusion d’images à chargement rapide de haute qualité.
