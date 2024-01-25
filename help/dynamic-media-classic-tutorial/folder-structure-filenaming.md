---
title: Définir votre structure de dossiers et votre convention de nommage des fichiers
description: Le nommage des fichiers est peut-être la décision la plus importante que vous prendrez lors de l’implémentation de Dynamic Media Classic. La structure de dossiers est également importante. Découvrez pourquoi ces éléments sont si importants et les approches possibles pour la structure de dossiers et les noms de fichiers.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
duration: 275
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1202'
ht-degree: 100%

---

# Définir votre structure de dossiers et votre convention de nommage des fichiers {#folder-structure-filenaming}

Avant de commencer à charger tout votre contenu, il est conseillé de tenir compte de la structure de dossiers que vous utiliserez, et notamment de votre convention de nommage des fichiers. Cela vous fera sans doute gagner du temps et vous évitera de répéter des tâches ultérieurement. Il est préférable de coordonner ces décisions entre tous les groupes.

## Hiérarchie des dossiers et convention de nommage des fichiers

Le nommage de fichier est généralement la décision la plus importante que vous prendrez concernant l’implémentation de Dynamic Media Classic. Toutefois, pour comprendre pourquoi ceci est si important, parlons tout d’abord de la structure de dossiers.

### Hiérarchie de dossiers

La hiérarchie de dossiers est importante pour vous et votre entreprise à des fins d’organisation uniquement : vos URL Dynamic Media Classic ne font référence qu’au nom de la ressource, et non au dossier ou au chemin d’accès. Où que vous ayez chargé un fichier, l’URL est la même. Cela diffère de la façon dont la plupart des gens organisent leurs images et contenus pour le web, mais avec Dynamic Media Classic, cela ne fait aucune différence.

Le nombre de ressources ou de dossiers à stocker dans chaque dossier est également important. Si de nombreuses ressources sont stockées dans un dossier, les performances se dégradent lors de l’affichage des ressources dans Dynamic Media Classic. Ne stockez pas des milliers de ressources dans un dossier. Développez plutôt une hiérarchie organisationnelle avec moins de 500 ressources ou dossiers au sein d’une branche donnée de votre hiérarchie. Il ne s’agit pas d’une exigence stricte, mais cela permet de maintenir des temps de réponse acceptables lors de l’affichage ou de la recherche de ressources. En effet, il est recommandé de créer des hiérarchies larges et peu profondes plutôt qu’étroites et profondes.

La méthode la plus simple pour créer des dossiers consiste à charger la structure complète de dossiers à l’aide de FTP et à activer l’option **Inclure les sous-dossiers**. Cette option permet à Dynamic Media Classic de recréer la structure de dossiers sur le site FTP dans Dynamic Media Classic.

Nous voulons que vous preniez en compte votre structure de dossiers avant de commencer à charger tous vos fichiers, car il est beaucoup plus facile d’organiser et de gérer vos fichiers et dossiers localement sur votre ordinateur que dans Dynamic Media Classic. Par exemple, dans Dynamic Media Classic, vous pouvez glisser-déposer des fichiers, mais pas des dossiers entiers.

### Stratégies de dossiers

Pour votre stratégie de dossiers, réfléchissez à ce qui est adapté à votre organisation. Voici quelques scénarios courants de nommage de dossiers :

- Site web miroir ou répartition de produit. Par exemple, si vous vendez des vêtements, vous pouvez avoir des dossiers Hommes, Femmes et Accessoires et des sous-dossiers Chemises et Chaussures.
- Stratégie basée sur le SKU ou l’ID du produit. Par exemple, pour les commerçantes et commerçants qui possèdent des milliers d’articles, il peut être logique d’utiliser des numéros de SKU ou des ID de produit comme noms de dossiers.
- Stratégie basée sur la marque. Par exemple, les fabricantes et les fabricants qui ont plusieurs marques peuvent choisir leurs noms de marque comme dossiers de premier niveau.

## Convention de nommage des fichiers

La manière dont vous choisissez de nommer vos fichiers est peut-être la décision préalable la plus importante que vous prendrez concernant Dynamic Media Classic. En effet, toutes les ressources de Dynamic Media Classic doivent porter des noms uniques, quel que soit l’emplacement de stockage dans le compte.

Toutes les URL et transactions de Dynamic Media Classic sont pilotées par un ID de ressource, qui est l’identifiant unique d’une ressource dans la base de données. Lorsque vous chargez un fichier, l’ID de ressource est créé en prenant le nom du fichier et en supprimant l’extension. Par exemple : _896649.jpg_ obtient l’ID de ressource _896649_.

Règles relatives aux ID de ressources :

- Deux ressources ne peuvent pas porter le même nom dans Dynamic Media Classic, quel que soit le dossier dans lequel se trouvent les ressources.
- Les noms respectent la casse. Par exemple, Chaise.jpg, chaise.jpg et CHAISE.jpg créent trois ID de ressource différents.
- Il est recommandé que les identifiants de ressources ne contiennent pas d’espaces ni de symboles. L’utilisation d’espaces et de symboles rend l’implémentation plus difficile, car vous devrez coder ces caractères dans l’URL. Par exemple, un espace «  » devient « %20 ».

Votre convention de nommage est en réalité la manière dont vous intégrez Dynamic Media Classic. En règle générale, vous n’intégrez pas vos systèmes back-office dans Dynamic Media Classic, car il s’agit d’un système fermé. C’est un partenaire passif, qui attend des instructions sous la forme d’URL.

La plupart des utilisateurs et utilisatrices basent leur convention de nommage sur leurs SKU internes ou leurs ID de produit, de sorte que lorsqu’une page web est appelée avec des informations sur ce SKU, la page peut automatiquement rechercher une image portant un nom similaire. S’il n’existe aucune connexion entre le nom de fichier et le SKU ou l’ID, votre système de back-office devra effectuer le suivi manuel de chaque nom de fichier, et une personne devra gérer ces associations. En bref, cela représente beaucoup de travail pour les équipes informatiques et de contenu.

### Stratégies de nommage de fichier

Votre stratégie de nommage doit être flexible pour une extension future, afin d’éviter d’avoir à renommer après le lancement. Voici quelques stratégies de nommage classiques :

**Aucune image alternative.** Dans ce scénario, vous n’avez qu’une image par produit et aucune vue alternative ou colorée. Vous attribuez un nom strict à chaque image en fonction de son SKU unique ou de son numéro d’ID de produit. Lors du chargement de la page, le modèle de page appelle l’ID de ressource avec le même numéro de SKU.

| SKU/ID de produit (PID) | Nom de fichier | ID de ressource |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

Il s’agit d’un système très simple, et approprié si vous avez des besoins modestes. Cependant, il n’est pas très flexible. Ce n’est pas parce que vous n’avez pas d’images alternatives aujourd’hui que vous ne les aurez pas demain. Le scénario suivant offre davantage de flexibilité.

**Stratégie basée sur l’image, les vues alternatives, les versions colorées, les nuanciers.** Cette stratégie permet d’obtenir des vues alternatives et/ou des vues colorées, si vous en avez. Plutôt que de nommer l’image selon le SKU uniquement, vous ajoutez un modificateur tel que « _1 » et « _2 » pour les vues alternatives, ainsi qu’un code couleur « _RED » ou « _BLU » pour les vues colorées. Si vous disposez à la fois d’images en couleur et d’autres vues pour le même produit, vous pouvez par exemple ajouter « _RED_1 » et « _RED_2 » pour la première et la deuxième vue de couleur rouge. Les nuanciers sont nommés avec le SKU, le code couleur et une extension « _SW » (Swatches).

| SKU/ID de produit (PID) | Catégorie | Nom de fichier | ID de ressource |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Vues alternatives | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|         | Vues colorées | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|         | Nuanciers | AA123_BLU_SW.tif | AA123_BLU_SW |
|         | Visionneuse d’images ou de nuanciers |                                             | AA123 ou AA123_SET | -- |

Lorsque vous travaillez avec des collections de visionneuses, telles que les visionneuses d’images et de nuanciers, la visionneuse elle-même doit également porter un nom unique. Ainsi, dans ce cas, la visionneuse peut se voir attribuer le SKU de base comme nom, ou le SKU avec une extension « _SET ».

### Convention de nommage et automatisation

Un dernier mot sur l’importance de la convention de nommage. Si vous souhaitez utiliser des visionneuses (telles que des visionneuses d’images ou de nuanciers), une convention de nommage prévisible vous permet d’automatiser leur création. Toute méthode par script, telle qu’un paramètre prédéfini de visionneuses par lots, dont vous allez découvrir les détails dans une autre section de ce tutoriel, peut s’appuyer sur une convention de nommage.

L’autre méthode consiste à créer manuellement vos visionneuses. Bien que la création manuelle de visionneuses d’images pour 200 images ne soit pas une tâche gigantesque, imaginez que vous ayez plus de 100 000 images. L’automatisation de la création des visionneuses devient alors cruciale.
