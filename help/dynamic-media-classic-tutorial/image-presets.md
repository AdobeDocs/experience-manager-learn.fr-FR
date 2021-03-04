---
title: Paramètres d’image prédéfinis
description: Paramètres d’image prédéfinis dans Dynamic Media Classic, vous disposez de tous les paramètres nécessaires pour créer une image à une taille, un format, une qualité et une accentuation spécifiques. Les paramètres d’image prédéfinis constituent un composant clé du dimensionnement dynamique. Lorsque vous consultez une URL dans Dynamic Media Classic, vous pouvez facilement déterminer si un paramètre d’image prédéfini est en cours d’utilisation. Découvrez les paramètres d’image prédéfinis, pourquoi ils sont si utiles et comment en créer un.
sub-product: dynamic-media
feature: Dynamic Media Classic, paramètres d’image prédéfinis
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Gestion de contenu
role: Professionnel
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 1%

---


# Paramètres d’image prédéfinis {#image-presets}

Un paramètre d’image prédéfini est essentiellement une recette qui contient tous les paramètres nécessaires pour créer une image à une taille, un format, une qualité et une accentuation spécifiques. Les paramètres d’image prédéfinis constituent un composant clé du dimensionnement dynamique.

Si vous regardez les URL d’à peu près n’importe quel client Dynamic Media Classic, vous verrez probablement un paramètre d’image prédéfini en cours d’utilisation. Il vous suffit de chercher $name$ à la fin de l&#39;URL (avec n&#39;importe quel mot ou mot remplacé par le nom).

Les paramètres d’image prédéfinis raccourcissent l’URL. Ainsi, au lieu d’écrire plusieurs instructions de diffusion d’images par requête, vous pouvez rédiger un seul paramètre d’image prédéfini. Par exemple, ces deux URL produisent la même image JPEG de 300 x 300 pixels avec accentuation, mais la seconde utilise un paramètre d’image prédéfini :

![image](assets/image-presets/image-preset-2.png)

La vraie valeur des paramètres d’image prédéfinis est que tout administrateur de Société peut mettre à jour la définition de ce paramètre d’image prédéfini et affecter chaque image utilisant ce format, sans modifier aucun code Web. Vous verrez les résultats de toute modification apportée à un paramètre d’image prédéfini après l’effacement du cache de l’URL.

>[!IMPORTANT]
>
>Lors du redimensionnement d’une image, les proportions, le rapport largeur/hauteur de l’image, doivent toujours être proportionnels afin que l’image ne soit pas déformée.

Un paramètre d’image prédéfini comporte un signe dollar ($) des deux côtés de son nom et suit le point d’interrogation (?) séparateur.

>[!TIP]
>
>Créez un paramètre d’image prédéfini par taille d’image unique sur votre site. Par exemple, si vous avez besoin d’une image de 350 x 350 pour la page des détails du produit, d’une image de 120 x 120 pour les pages de navigation/recherche et d’une image de 90 x 90 pour un article de vente croisée/incitatif, vous avez besoin de trois paramètres d’image prédéfinis, que vous disposiez de 5000 ou 50 0 0 0 0 0 000 00000000 000 000000000000000000000000000000000000000000000000000000000

- En savoir plus sur les [paramètres d’image prédéfinis](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- Découvrez comment [créer un paramètre d’image prédéfini](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## Paramètres d’image prédéfinis et accentuation

En règle générale, les paramètres d’image prédéfinis redimensionnent une image ; chaque fois que vous redimensionnez une image à partir de sa taille d’origine, vous devez ajouter de l’accentuation. Cela est dû au fait que le redimensionnement entraîne la fusion et la fusion de nombreux pixels dans un espace plus petit, ce qui rend l’image douce et floue. L’accentuation augmente le contraste des bords et des zones de contraste élevé dans une image.

Nous nous attendons à ce que les images haute résolution que vous téléchargez dans Dynamic Media Classic n’aient pas besoin d’être accentuées lorsqu’elles sont visualisées en taille réelle — lorsque vous effectuez un zoom avant. Cependant, à toute taille plus petite, une certaine accentuation est généralement souhaitable.

>[!TIP]
>
>Toujours accentuer lors du redimensionnement des images ! Cela signifie que vous devrez ajouter l’accentuation à chaque paramètre d’image prédéfini (et à chaque paramètre prédéfini de visionneuse, dont nous parlerons plus tard).
>
>Si vos images n&#39;ont pas l&#39;air bonnes, il y a de fortes chances qu&#39;elles aient besoin d&#39;accentuation ou peut-être que la qualité était médiocre pour commencer.

Le degré d’accentuation à ajouter est entièrement subjectif. Certaines personnes aiment les images plus douces, tandis que d&#39;autres les apprécient très nettement. Il est facile d’améliorer une image en exécutant une combinaison de filtres d’accentuation sur une image. Cependant, il est également facile de passer par-dessus bord et de trop accentuer une image.

Le graphique suivant présente trois niveaux d’accentuation. De droite à gauche, vous n&#39;avez pas d&#39;accentuation, juste la bonne quantité, et trop.

![image](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic permet trois types d’accentuation : Accentuation simple, mode de ré-échantillonnage et masquage flou.

En savoir plus sur [Options d’accentuation classique de Dynamic Media](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## Ressources supplémentaires

[Guide](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf) des paramètres d’image prédéfinis. Paramètres à utiliser pour optimiser la qualité d’image et la vitesse de chargement.

[Image Is Everything Part 2 : Ce n&#39;est jamais juste un flou — la qualité contre la vitesse](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). Publication de blog traitant de l’utilisation des paramètres d’image prédéfinis pour la diffusion d’images à chargement rapide et de haute qualité.

[L’Image Est Tout Ce Qui Est Des Webinaires](https://dynamicmediaseries2019.enterprise.adobeevents.com/). Liens vers les enregistrements des trois webinars de la série _Image Is Everything_. [Le webinaire 2](https://seminars.adobeconnect.com/p6lqaotpjnd3) traite des paramètres d’image prédéfinis.
