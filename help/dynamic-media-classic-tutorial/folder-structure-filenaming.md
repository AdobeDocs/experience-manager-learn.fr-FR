---
title: Détermination de la structure de dossiers et de la convention d’attribution de noms de fichiers
description: Le nommage de fichiers est peut-être la décision la plus importante que vous prendrez lors de l’implémentation de Dynamic Media Classic. La structure des dossiers est également importante. Découvrez pourquoi il s'agit d'approches si importantes et possibles pour la structure de dossiers et les noms de fichiers.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
topic: Gestion de contenu
role: Professionnel
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1219'
ht-degree: 0%

---


# Détermination de la structure de votre dossier et de la convention d&#39;appellation des fichiers {#folder-structure-filenaming}

Avant d’accéder au contenu et de le télécharger par début, il est recommandé de tenir compte de la structure de dossiers que vous utiliserez et en particulier de la convention d’affectation des noms de fichier. Cela vous fera sans doute gagner du temps et vous obligera à refaire des tâches plus tard. Il est préférable de coordonner ces décisions entre tous les groupes.

## Convention sur la hiérarchie des dossiers et le nommage de fichiers

Le nommage de fichiers est généralement la décision la plus importante que vous prenez concernant l’implémentation de Dynamic Media Classic. Toutefois, pour comprendre pourquoi il est important, parlons tout d’abord de la structure de dossiers.

### Hiérarchie des dossiers

La hiérarchie des dossiers est importante pour vous et votre société à des fins d’organisation uniquement : vos URL Dynamic Media Classic référencent uniquement le nom de la ressource, et non le dossier ou le chemin d’accès. Où que vous téléchargiez un fichier, l’URL sera la même. Cela diffère beaucoup de la façon dont la plupart des gens organisent leurs images et leurs contenus pour le web, mais avec Dynamic Media Classic, cela ne fait aucune différence.

Le nombre de fichiers ou de dossiers à stocker dans chaque dossier est une autre considération importante. Si de nombreux fichiers sont stockés dans un dossier, les performances se dégradent lors de l’affichage des fichiers dans Dynamic Media Classic. Ne stockez pas des milliers de fichiers dans un dossier. Développez plutôt une hiérarchie organisationnelle contenant moins de 500 fichiers ou dossiers au sein d’une branche donnée de votre hiérarchie. Il ne s’agit pas d’une exigence stricte, mais elle permet de maintenir des temps de réponse acceptables lors de l’affichage ou de la recherche de ressources. En effet, la recommandation consiste à créer des hiérarchies larges et peu profondes plutôt que étroites et profondes.

Le moyen le plus simple de créer vos dossiers consiste à télécharger la structure complète de dossiers à l’aide du protocole FTP et à activer l’option **Inclure les sous-dossiers**. Avec cette option, Dynamic Media Classic recrée la structure de dossiers sur le site FTP dans Dynamic Media Classic.

Nous voulons que vous preniez en compte votre structure de dossiers avant de début télécharger tous vos fichiers, car il est beaucoup plus facile d&#39;organiser et de gérer vos fichiers et dossiers localement sur votre ordinateur que dans Dynamic Media Classic. Par exemple, vous pouvez uniquement faire glisser des fichiers, mais pas des dossiers entiers, dans Dynamic Media Classic.

### Stratégies de dossiers

Pour votre stratégie de dossiers, tenez compte de ce qui a du sens pour votre entreprise. Voici quelques scénarios d’attribution de noms de dossiers courants :

- Mettre en miroir la ventilation du site Web ou du produit. Par exemple, si vous avez vendu des vêtements, vous pouvez avoir des dossiers pour hommes, femmes et accessoires et des sous-dossiers pour Chemises et chaussures.
- Stratégie basée sur le SKU ou l’ID de produit. Par exemple, avec les détaillants qui possèdent des milliers d’éléments, il peut être logique d’utiliser des numéros de SKU ou des ID de produit comme noms de dossiers.
- Stratégie de marque. Par exemple, les fabricants qui ont plusieurs marques peuvent choisir leurs noms de marque comme dossiers de niveau supérieur.

## Convention de dénomination de fichier

La façon dont vous choisissez de nommer vos fichiers est peut-être la décision la plus importante que vous prendrez au sujet de Dynamic Media Classic. En effet, tous les fichiers de Dynamic Media Classic doivent avoir des noms uniques, quel que soit leur emplacement de stockage dans le compte.

Toutes les URL et transactions de Dynamic Media Classic sont gérées par un ID de ressource, qui est l’identifiant unique d’un élément dans la base de données. Lorsque vous téléchargez un fichier, l’ID de fichier est créé en prenant le nom de fichier et en supprimant l’extension. Par exemple, _896649.jpg_ obtient l’actif _ID 896649_.

Règles relatives aux ID de fichier :

- Aucun fichier ne peut porter le même nom dans Dynamic Media Classic, quel que soit le dossier dans lequel il se trouve.
- Les noms respectent la casse. Par exemple, les fichiers Chaise.jpg, Chaise.jpg et CHAIR.jpg créeront trois identifiants de ressource différents.
- En règle générale, les ID de fichier ne doivent pas contenir d’espaces ou de symboles vides. L’utilisation d’espaces et de symboles rend la mise en oeuvre plus difficile car vous devrez coder ces caractères dans une URL. Par exemple, un espace &quot;&quot; devient &quot;%20&quot;.

Votre convention d’affectation de nom est essentiellement la manière dont vous vous intégrez à Dynamic Media Classic. En règle générale, vous n’intégrez pas vos systèmes back-office dans Dynamic Media Classic, car il s’agit d’un système fermé. Il s’agit d’un partenaire passif, qui attend des instructions sous forme d’URL.

La plupart des utilisateurs basent leur convention d’affectation de nom sur leur SKU interne ou leur ID de produit de sorte que lorsqu’une page Web est appelée avec des informations sur ce SKU, la page peut automatiquement rechercher une image portant le même nom. S&#39;il n&#39;y a pas de connexion entre le nom de fichier et le SKU ou l&#39;ID, votre système d&#39;arrière-plan devra effectuer manuellement le suivi de chaque nom de fichier et une personne devra gérer ces associations — en bref, beaucoup de travail pour les équipes informatique et de contenu.

### Stratégies de nommage de fichiers

Votre stratégie d’attribution de noms doit être flexible pour une future expansion. Vous pouvez donc éviter d’avoir à renommer une fois lancée. Voici quelques stratégies d’attribution de noms typiques :

**Aucune image de remplacement.** Dans ce scénario, vous n’avez qu’une image par produit et aucune vue alternative ou colorée. Vous attribuez un nom strict à chaque image en fonction de son SKU unique ou de son numéro d’ID de produit. Lors du chargement de la page, le modèle de page appelle l’ID de fichier avec le même numéro de SKU.

| SKU/PID | Nom de fichier | ID de ressource |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

C&#39;est un système très simple, et bon si vous avez des besoins modestes. Cependant, elle n&#39;est pas très flexible. Ce n&#39;est pas parce que vous n&#39;avez pas d&#39;images alternatives aujourd&#39;hui que vous n&#39;en aurez pas demain. Le scénario suivant offre davantage de flexibilité.

**A l’aide de l’image, d’autres vues, versions colorées, nuances.** Cette stratégie permet d&#39;autres vues et/ou vues colorées, si vous en avez. Plutôt que de nommer l’image après le SKU uniquement, vous ajoutez un modificateur tel que &quot;_1&quot; et &quot;_2&quot; pour les vues de remplacement et un code de couleur de &quot;_RED&quot; ou &quot;_BLU&quot; pour les vues de couleur. Si vous disposez à la fois d’images colorées et de vues de remplacement pour le même produit, vous pouvez ajouter &quot;_RED_1&quot; et &quot;_RED_2&quot; pour la première et la deuxième vue de couleur rouge. Les nuanciers sont nommés avec le SKU, le code couleur et une extension &quot;_SW&quot;.

| SKU/PID | Catégorie | Nom de fichier | ID de ressource |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Alt vues | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Vues colorées | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Échantillons | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Visionneuse d’images ou de série d’échantillons |  | AA123 ou AA123_SET | — |

Lorsqu’il s’agit de collections définies, telles que les visionneuses d’images et les séries d’échantillons, la visionneuse elle-même doit également porter un nom unique. Dans ce cas, l’ensemble peut recevoir le SKU de base comme nom, ou le SKU avec une extension &quot;_SET&quot;.

### Convention de dénomination et automatisation

Un dernier mot sur l&#39;importance de la convention de dénomination. Si vous souhaitez utiliser des visionneuses (telles que les visionneuses d’images ou les séries d’échantillons), une convention d’affectation de nom prévisible vous permet d’automatiser leur création. Toute méthode par script, telle qu’un paramètre prédéfini d’ensemble par lot, que vous trouverez dans une section distincte de ce didacticiel, peut être éliminée d’une convention d’affectation de nom.

L&#39;autre méthode consiste à créer manuellement vos visionneuses. Bien que la création manuelle de visionneuses d’images pour 200 images ne soit pas une tâche aisée, imaginez que vous ayez plus de 100 000 images. C&#39;est là que l&#39;automatisation de la création des jeux devient cruciale.
