---
title: Paramètres prédéfinis d’image
description: Les paramètres d’image prédéfinis dans Dynamic Media Classic contiennent tous les paramètres nécessaires pour créer une image à une taille, un format, une qualité et une accentuation spécifiques. Les paramètres prédéfinis d’image sont un composant clé du dimensionnement dynamique. Lorsque vous examinez une URL dans Dynamic Media Classic, vous pouvez facilement voir si un paramètre prédéfini d’image est en cours d’utilisation. Découvrez les paramètres prédéfinis d’image, pourquoi ils sont si utiles et comment en créer un.
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 164
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 100%

---

# Paramètres prédéfinis d’image {#image-presets}

Un paramètre prédéfini d’image est essentiellement une recette qui contient tous les paramètres nécessaires pour créer une image à une taille, un format, une qualité et une accentuation spécifiques. Les paramètres prédéfinis d’image sont un composant clé du dimensionnement dynamique.

Si vous examinez les URL de pratiquement n’importe quelle personne cliente de Dynamic Media Classic, vous verrez probablement un paramètre prédéfini d’image en cours d’utilisation. Il vous suffit de rechercher $name$ à la fin de l’URL (avec un ou plusieurs mots remplaçant « name »).

Les paramètres prédéfinis d’image raccourcissent l’URL. Ainsi, au lieu d’écrire plusieurs instructions de diffusion d’images par requête, vous pouvez écrire un seul paramètre prédéfini d’image. Par exemple, ces deux URL produisent la même image JPEG de 300x300 avec accentuation, mais la seconde utilise un paramètre prédéfini d’image :

![image](assets/image-presets/image-preset-2.png)

La valeur réelle des paramètres prédéfinis d’image est que toute personne chargée de l’administration d’entreprise peut mettre à jour la définition de ce paramètre prédéfini d’image et affecter chaque image utilisant ce format, sans modifier de code web. Vous verrez les résultats de toute modification apportée à un paramètre prédéfini d’image après l’effacement du cache de l’URL.

>[!IMPORTANT]
>
>Lors du redimensionnement d’une image, le format, le rapport largeur/hauteur de l’image, doit toujours être maintenu proportionnel afin que l’image ne soit pas déformée.

Un paramètre prédéfini d’image comporte un symbole dollar ($) des deux côtés de son nom et suit le point d’interrogation (?). séparateur.

>[!TIP]
>
>Créez un paramètre prédéfini d’image par taille d’image unique sur votre site. Par exemple, si vous avez besoin d’une image de 350x350 pour la page des détails du produit, d’une image de 120x120 pour les pages de navigation/recherche et d’une image de 90x90 pour une vente croisée/un article présenté, il vous faut trois paramètres prédéfinis d’image, que vous ayez 500 images ou 500 000.

- En savoir plus sur les [Paramètres prédéfinis d’image](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html?lang=fr).
- Découvrir comment [Créer un paramètre prédéfini d’image](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html?lang=fr#creating-an-image-preset).

## Paramètres prédéfinis d’image et accentuation

Les paramètres prédéfinis d’image redimensionnent généralement une image. Dès que vous redimensionnez une image à partir de sa taille d’origine, il est judicieux d’ajouter une accentuation. Cela est dû au fait que le redimensionnement entraîne la fusion de nombreux pixels dans un espace plus petit. Résultat, l’image devient floue. L’accentuation augmente le contraste des bords et des zones à fort contraste dans une image.

Il est probable que les images haute résolution que vous chargez dans Dynamic Media Classic n’ont pas besoin d’accentuation lorsqu’elles sont affichées en taille réelle, lors du zoom avant. Toutefois, à une taille plus petite, une accentuation est généralement souhaitable.

>[!TIP]
>
>Accentuez toujours lorsque vous redimensionnez des images. Cela signifie que vous devrez ajouter une accentuation à chaque paramètre prédéfini d’image (et au paramètre prédéfini de la visionneuse, dont nous parlerons plus loin).
>
>Si vos images ne sont pas belles, c’est probablement parce qu’elles ont besoin d’une accentuation, ou peut-être que la qualité était médiocre à la base.

Le degré de l’accentuation à ajouter est entièrement subjectif. Certaines personnes aiment les images plus floues, tandis que d’autres les apprécient très nettes. Il est facile d’améliorer une image en exécutant une combinaison de filtres d’accentuation sur une image. Cependant, il est également facile de surcharger et d’accentuer une image de manière excessive.

Le graphique suivant montre trois niveaux d’accentuation. De droite à gauche, les images présentent d’abord l’absence d’accentuation, puis la quantité idéale, et enfin une quantité excessive.

![image](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic permet trois types d’accentuation : accentuation simple, mode rééchantillonnage et accentuation.

En savoir plus sur les [Options d’accentuation de Dynamic Media Classic](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html?lang=fr#sharpening_an_image).

## Ressources supplémentaires

[Guide des paramètres prédéfinis d’image](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). Paramètres à utiliser pour optimiser la qualité des images et la vitesse de chargement.

[Image Is Everything partie 2 : ce n’est jamais qu’un flou — Qualité ou vitesse](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Article de blog traitant de l’utilisation des paramètres prédéfinis d’image pour la diffusion d’images à chargement rapide de haute qualité.
