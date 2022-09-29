---
title: Détermination de la structure de dossiers et de la convention d’appellation des fichiers
description: La dénomination de fichier est peut-être la décision la plus importante que vous prendrez lors de l’implémentation de Dynamic Media Classic. La structure de dossiers est également importante. Découvrez pourquoi il est si important et possible d’utiliser des approches pour la structure de dossiers et les noms de fichiers.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
activity: develop
topics: development, authoring, configuring, architecture
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: 15121896-9196-4ce0-aff2-9178563326b4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 0%

---

# Détermination de la structure de dossiers et de la convention d’appellation des fichiers {#folder-structure-filenaming}

Avant d’intervenir et de commencer à charger tout votre contenu, il est conseillé de tenir compte de la structure de dossiers que vous utiliserez, et notamment de votre convention d’appellation des fichiers. Cela vous fera sans doute gagner du temps et vous obligera à rétablir les tâches ultérieurement. Il est préférable de coordonner ces décisions entre tous les groupes.

## Hiérarchie des dossiers et convention de dénomination des fichiers

L’attribution de noms de fichier est généralement la décision la plus importante que vous prenez concernant l’implémentation de Dynamic Media Classic. Toutefois, pour comprendre pourquoi il est important, parlons tout d’abord de la structure de dossiers.

### Hiérarchie de dossiers

La hiérarchie de dossiers est importante pour vous et votre entreprise à des fins d’organisation uniquement : vos URL Dynamic Media Classic ne font référence qu’au nom de la ressource, et non au dossier ou au chemin d’accès. Où que vous ayez téléchargé un fichier, l’URL est la même. Cela diffère de la façon dont la plupart des gens organisent leurs images et contenus pour le web, mais avec Dynamic Media Classic, cela ne fait aucune différence.

Le nombre de ressources ou de dossiers à stocker dans chaque dossier constitue une autre considération importante. Si de nombreuses ressources sont stockées dans un dossier, les performances se dégradent lors de l’affichage des ressources dans Dynamic Media Classic. Ne stockez pas des milliers de ressources dans un dossier. Développez plutôt une hiérarchie organisationnelle avec moins de 500 ressources ou dossiers au sein d’une branche donnée de votre hiérarchie. Il ne s’agit pas d’une exigence stricte, mais cela permet de maintenir des temps de réponse acceptables lors de l’affichage ou de la recherche de ressources. En effet, la recommandation consiste à créer des hiérarchies larges et superficielles plutôt que étroites et profondes.

La méthode la plus simple pour créer des dossiers consiste à charger la structure complète de dossiers à l’aide de FTP et à activer l’option . **Inclure les sous-dossiers**. Cette option entraîne Dynamic Media Classic à recréer la structure de dossiers sur le site FTP dans Dynamic Media Classic.

Nous voulons que vous preniez en compte la structure de vos dossiers avant de commencer à charger tous vos fichiers, car il est beaucoup plus facile d’organiser et de gérer vos fichiers et dossiers localement sur votre ordinateur que dans Dynamic Media Classic. Par exemple, vous pouvez uniquement faire glisser des fichiers, mais pas des dossiers entiers, dans Dynamic Media Classic.

### Stratégies de dossiers

Pour votre stratégie de dossiers, tenez compte de ce qui a du sens pour votre entreprise. Voici quelques scénarios courants d’attribution de noms aux dossiers :

- Site web miroir ou ventilation de produit. Par exemple, si vous avez vendu des vêtements, vous pouvez disposer de dossiers pour Hommes, Femmes et Accessoires, et de sous-dossiers pour Chemises et Chaussures.
- Stratégie basée sur le SKU ou l’ID de produit. Par exemple, avec les détaillants qui possèdent des milliers d’articles, il peut être logique d’utiliser des numéros de SKU ou des ID de produit comme noms de dossiers.
- Stratégie de marque. Par exemple, les fabricants qui ont plusieurs marques peuvent choisir leurs noms de marque comme dossiers de niveau supérieur.

## Convention d’appellation des fichiers

La manière dont vous choisissez de nommer vos fichiers est peut-être la décision anticipée la plus importante que vous prendrez concernant Dynamic Media Classic. En effet, toutes les ressources de Dynamic Media Classic doivent porter des noms uniques, quel que soit l’emplacement de stockage dans le compte.

Toutes les URL et transactions de Dynamic Media Classic sont pilotées par un ID de ressource, qui est l’identifiant unique d’une ressource dans la base de données. Lorsque vous chargez un fichier, l’ID de ressource est créé en prenant le nom du fichier et en supprimant l’extension. Par exemple : _896649.jpg_ Obtenir une ressource _ID 896649_.

Règles relatives aux ID de ressources :

- Aucune ressource ne peut porter le même nom dans Dynamic Media Classic, quel que soit le dossier dans lequel se trouvent les ressources.
- Les noms sont sensibles à la casse. Par exemple, Chaise.jpg, chaise.jpg et CHAIR.jpg créent trois identifiants de ressource différents.
- Il est recommandé que les identifiants de ressources ne contiennent pas d’espaces ni de symboles vides. L’utilisation d’espaces et de symboles rend la mise en oeuvre plus difficile, car vous devrez coder ces caractères dans l’URL. Par exemple, un espace &quot;&quot; devient &quot;%20&quot;.

Votre convention d’affectation des noms est essentiellement la manière dont vous intégrez Dynamic Media Classic. En règle générale, vous n’intégrez pas vos systèmes administratifs dans Dynamic Media Classic, car il s’agit d’un système fermé. C&#39;est un partenaire passif, qui attend des instructions sous la forme d&#39;URL.

La plupart des utilisateurs basent leur convention d’affectation de nom autour de leur SKU interne ou de leurs ID de produit, de sorte que lorsqu’une page web est appelée avec des informations sur ce SKU, la page peut automatiquement rechercher une image portant un nom similaire. S’il n’existe aucune connexion entre le nom de fichier et le SKU ou l’ID, votre système de back-office devra effectuer manuellement le suivi de chaque nom de fichier, et une personne devra gérer ces associations — en bref, beaucoup de travail pour les équipes informatiques et de contenu.

### Stratégies de dénomination de fichier

Votre stratégie de dénomination doit être flexible pour une extension ultérieure. Vous pouvez donc éviter d’avoir à renommer une fois que vous avez lancé. Voici quelques stratégies de dénomination classiques :

**Aucune image alternative.** Dans ce scénario, vous n’avez qu’une image par produit et aucune vue alternative ou colorée. Vous attribuez un nom strict à chaque image en fonction de son SKU unique ou de son numéro d’ID de produit. Au chargement de la page, le modèle de page appelle l’ID de ressource avec le même numéro de SKU.

| SKU/PID | Nom de fichier | ID de ressource |
| ------- | ---------- | -------- |
| 896649 | 896649.jpg | 896649 |
| SKU123 | SKU123.png | SKU123 |

C&#39;est un système très simple, et bon si vous avez des besoins modestes. Cependant, il n&#39;est pas très flexible. Ce n&#39;est pas parce que vous n&#39;avez pas d&#39;images alternatives aujourd&#39;hui que vous n&#39;aurez pas ces images demain. Le scénario suivant offre davantage de flexibilité.

**À l’aide de l’image, des vues alternatives, des versions colorées, des échantillons.** Cette stratégie permet d’obtenir des vues alternatives et/ou colorées, si vous en avez. Plutôt que de nommer l’image après le SKU uniquement, vous ajoutez un modificateur tel que &quot;_1&quot; et &quot;_2&quot; pour les vues de remplacement, ainsi qu’un code couleur &quot;_RED&quot; ou &quot;_BLU&quot; pour les vues colorées. Si vous disposez à la fois d’images en couleur et d’autres vues pour le même produit, vous pouvez peut-être ajouter &quot;_RED_1&quot; et &quot;_RED_2&quot; pour la première et la deuxième vue de couleur rouge. Les nuanciers sont nommés avec le SKU, le code couleur et une extension &quot;_SW&quot;.

| SKU/PID | Catégorie | Nom de fichier | ID de ressource |
| ------- | ----------------------- | ------------------------------------------- | ------------------------------- |
| AA123 | Vues Alt | AA123_1.tif AA123_2.tif AA123_3.tif | AA123_1 AA123_2 AA123_3 |
|  | Vues colorées | AA123_BLU.tif AA123_RED.tif AA123_BROWN.tif | AA123_BLU AA123_RED AA123_BROWN |
|  | Échantillons | AA123_BLU_SW.tif | AA123_BLU_SW |
|  | Visionneuse d’images ou d’échantillons |  | AA123 ou AA123_SET | — |

Lorsque vous traitez de collections d’ensembles, telles que les visionneuses d’images et d’échantillons, la visionneuse elle-même doit également porter un nom unique. Ainsi, dans ce cas, l’ensemble peut se voir attribuer le SKU de base comme nom, ou le SKU avec une extension &quot;_SET&quot;.

### Convention d’affectation de nom et automatisation

Un dernier mot sur l&#39;importance de la convention de dénomination. Si vous souhaitez utiliser des visionneuses (telles que des visionneuses d’images ou des séries d’échantillons), une convention d’affectation de nom prévisible vous permet d’automatiser leur création. Toute méthode par script, telle qu’un paramètre prédéfini d’ensemble par lot, dont vous allez découvrir les détails dans une section distincte de ce tutoriel, peut être supprimée d’une convention d’affectation des noms.

L’autre méthode consiste à créer manuellement vos visionneuses. Bien que la création manuelle de visionneuses d’images pour 200 images ne soit pas une tâche gigantesque, imaginez que vous ayez plus de 100 000 images. C&#39;est là que l&#39;automatisation de la création des jeux devient cruciale.
