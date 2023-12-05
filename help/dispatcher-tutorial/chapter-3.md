---
title: Chapitre 3 - Rubriques de mise en cache du Dispatcher avancé
description: Il s’agit de la partie 3 d’une série en trois parties sur la mise en cache dans AEM. Les deux premières parties portaient sur la mise en cache HTTP simple dans le Dispatcher et leurs limites. Cette partie présente quelques idées sur la manière de surmonter ces limites.
feature: Dispatcher
topic: Architecture
role: Architect
level: Intermediate
doc-type: Tutorial
exl-id: 7c7df08d-02a7-4548-96c0-98e27bcbc49b
duration: 1674
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '6172'
ht-degree: 99%

---

# 3 - Analyse avancée de la mise en cache

*« Il n’y a que deux choses difficiles dans l’informatique : invalider le cache et nommer les choses. »*

— PHIL KARLTON

## Vue d’ensemble

Il s’agit de la partie 3 d’une série en trois parties sur la mise en cache dans AEM. Les deux premières parties portaient sur la mise en cache HTTP simple dans le Dispatcher et leurs limites. Cette partie présente quelques idées sur la manière de surmonter ces limites.

## Mise en cache en général

Le [chapitre 1](chapter-1.md) et le [chapitre 2](chapter-2.md) de cette série concernaient principalement le Dispatcher. Nous avons expliqué les principes de base, les limites et les compromis à faire.

La complexité et les subtilités de la mise en cache ne sont pas des problèmes propres au Dispatcher. La mise en cache est difficile en général.

Si le Dispatcher était votre seul outil dans votre boîte à outils, cela constituerait une véritable limitation.

Dans ce chapitre, nous voulons élargir notre approche de la mise en cache et développer certaines idées sur la manière de surmonter des lacunes de Dispatcher. Il n’y a pas de solution miracle, vous devrez faire des compromis dans votre projet. Souvenez-vous qu’avec la mise en cache et l’invalidation, la précision devient toujours complexe, et que la complexité s’accompagne de la possibilité de faire des erreurs.

Vous devrez faire des compromis dans ces domaines :

* Performances et latence
* Utilisation des ressources/Charge du processeur/Utilisation du disque
* Précision/Actualité/Obsolescence/Sécurité
* Simplicité/Complexité/Coût/Maintenabilité/Tendance aux erreurs

Ces dimensions sont interconnectées dans un système plutôt complexe. Il n’existe pas de simple si-ceci-alors-cela. Rendre un système plus simple peut le rendre plus rapide ou plus lent. Cela peut réduire vos coûts de développement, mais augmenter les coûts au niveau du service d’assistance, par exemple si les clientes ou clients voient du contenu obsolète ou se plaignent d’un site web lent. Tous ces facteurs doivent être pris en compte et équilibrés les uns par rapport aux autres. Mais à l’heure actuelle, vous devriez déjà savoir qu’il n’existe pas de solution miracle ou une seule bonne pratique ; seulement de nombreuses mauvaises pratiques et quelques bonnes.

## Mise en cache en chaîne

### Vue d’ensemble

#### Flux de données

La diffusion d’une page d’un serveur vers le navigateur d’un client ou d’une cliente traverse une multitude de systèmes et de sous-systèmes. Si vous regardez attentivement, il existe un certain nombre de données de sauts à transférer de la source vers le drain, chacune d’elles étant un candidat potentiel pour la mise en cache.

![Flux de données d’une application CMS standard.](assets/chapter-3/data-flow-typical-cms-app.png)

*Flux de données d’une application CMS standard.*

<br> 

Commençons notre parcours avec une donnée qui se trouve sur un disque dur et qui doit être affichée dans un navigateur.

#### Matériel et système d’exploitation

Tout d’abord, le disque dur (HDD) lui-même dispose d’un cache intégré dans le matériel. Deuxièmement, le système d’exploitation qui monte le disque dur utilise la mémoire libre pour mettre en cache les blocs fréquemment utilisés afin d’accélérer l’accès.

#### Référentiel de contenu

Le niveau suivant est le CRX ou Oak, la base de données de documents utilisée par AEM. CRX et Oak divisent les données en segments pouvant être mis en cache en mémoire, afin d’éviter un accès plus lent au HDD.

#### Données tierces

La plupart des grandes installations web disposent également de données tierces ; des données provenant d’un système d’informations sur les produits, un système de gestion des relations client, une base de données héritée ou tout autre service web arbitraire. Ces données n’ont pas besoin d’être extraites de la source à chaque fois que l’on en a besoin, surtout lorsqu’on sait qu’elles changent peu fréquemment. Elles peuvent donc être mises en cache si elles ne sont pas synchronisées dans la base de données CRX.

#### Couche métier - Application/modèle

En règle générale, vos scripts de modèle n’effectuent pas le rendu du contenu brut provenant de CRX via l’API JCR. Il est probable que vous ayez une couche métier entre les deux qui fusionne, calcule et/ou transforme les données dans un objet de domaine professionnel. Devinez quoi ? Si ces opérations sont coûteuses, envisagez de les mettre en cache.

#### Fragments de balisage

Le modèle est désormais la base du rendu du balisage d’un composant. Pourquoi ne pas également mettre en cache le modèle rendu ?

#### Dispatcher, réseau CDN et autres proxys

La page HTML rendue est envoyée au Dispatcher. Nous avons déjà discuté du fait que le principal objectif du Dispatcher est de mettre en cache les pages HTML et d’autres ressources web (malgré son nom). Avant que les ressources n’atteignent le navigateur, il peut transmettre un proxy inverse, qui peut mettre en cache et un réseau CDN, également utilisé pour la mise en cache. Le client ou la cliente peut se trouver dans un bureau qui n’accorde l’accès au web qu’au moyen d’un proxy, et ce proxy peut décider de mettre en cache également pour économiser du trafic.

#### Mémoire cache du navigateur

Dernier point, mais non des moindres : le navigateur est également mis en cache. Il s’agit d’une ressource que l’on néglige facilement. Mais il s’agit du cache le plus proche et le plus rapide de la chaîne de mise en cache. Malheureusement, il n’est pas partagé entre les utilisateurs et utilisatrices, mais toujours entre différentes demandes d’un utilisateur ou d’une utilisatrice.

### Où mettre en cache et pourquoi ?

Il s’agit d’une longue chaîne de mémoires cache potentielles. Et nous avons tous rencontré des problèmes avec des contenus obsolètes. Mais en prenant en compte le nombre d’étapes, c’est un miracle que cela fonctionne la plupart du temps.

Mais à quelle étape de cette chaîne est-il logique de mettre en cache ? Au début ? À la fin ? Tout le temps ? Ça dépend d’un grand nombre de facteurs. Même deux ressources d’un même site web peuvent nécessiter une réponse différente à cette question.

Pour vous donner une idée approximative des facteurs que vous devez prendre en compte :

**Durée de vie** : si la durée de vie des objets est courte (la durée de vie des données de trafic peut être plus courte que celle des données météorologiques), il peut ne pas être utile de les mettre en cache.

**Coût de production** : la reproduction et la diffusion d’un objet peuvent être très coûteuses (en termes de cycles de processeur et d’E/S). Si elles ne sont pas très coûteuses, la mise en cache peut ne pas être nécessaire.

**Taille** : les objets volumineux nécessitent davantage de ressources pour être mis en cache. Cela pourrait être un facteur limitatif et doit être comparé aux avantages.

**Fréquence d’accès** : si les objets sont rarement accessibles, la mise en cache peut ne pas être efficace. Ils deviennent obsolètes ou sont invalidés avant que l’on y accède la deuxième fois à partir du cache. De tels éléments bloqueraient les ressources de mémoire.

**Accès partagé** : les données utilisées par plusieurs entités doivent être mises en cache plus haut dans la chaîne. En fait, la chaîne de mise en cache n’est pas une chaîne, mais une arborescence. Une donnée du référentiel peut être utilisée par plusieurs modèles. Ces modèles peuvent à leur tour être utilisés par plusieurs scripts de rendu pour générer des fragments de HTML. Ces fragments sont inclus dans plusieurs pages qui sont distribuées à plusieurs utilisateurs ou utilisatrices avec leurs caches privés dans le navigateur. Donc « partager » ne signifie pas partager seulement entre des personnes, mais plutôt entre des morceaux de logiciels. Si vous souhaitez trouver un cache « partagé » potentiel, remontez simplement l’arborescence vers la racine et recherchez un ancêtre commun, c’est là que vous devez mettre en cache.

**Répartition géographique** : si vos utilisateurs et utilisatrices sont répartis dans le monde entier, l’utilisation d’un réseau de caches distribué peut contribuer à réduire la latence.

**Bande passante réseau et latence** : en parlant de latence, qui sont vos clientes et clients et quel type de réseau utilisent-ils ? Peut-être qu’il s’agit de clientes et clients mobiles dans un pays sous-développé qui utilisent la connexion 3G de smartphones de génération plus ancienne ? Envisagez de créer des objets plus petits et de les mettre en cache dans les caches du navigateur.

Cette liste est loin d’être exhaustive, mais nous pensons que vous avez maintenant compris l’idée.

### Règles de base pour la mise en cache en chaîne

Encore une fois : la mise en cache est difficile. Partageons quelques règles de base que nous avons extraites de projets précédents qui peuvent vous aider à éviter des problèmes dans votre projet.

#### Éviter la double mise en cache

Chacune des couches introduites dans le dernier chapitre apporte une certaine valeur dans la chaîne de mise en cache. Soit en économisant les cycles de calcul, soit en rapprochant les données des consommateurs et consommatrices. Ce n’est pas une mauvaise chose de mettre en cache une donnée à plusieurs étapes de la chaîne, mais vous devez toujours tenir compte des avantages et des coûts de la prochaine étape. La mise en cache d’une page entière dans le système de publication n’offre généralement aucun avantage, étant donné qu’elle est déjà faite dans Dispatcher.

#### Mélanger des stratégies d’invalidation

Il existe trois stratégies d’invalidation de base :

* **TTL, Time to Live :** un objet expire après un délai fixe (par exemple, « dans 2 heures »).
* **Date d’expiration :** l’objet expire au moment défini (par exemple, « 17 h 00 le 10 juin 2019 »).
* **Basée sur un événement :** l’objet est invalidé explicitement par un événement qui s’est produit dans la plateforme (par exemple, lorsqu’une page est modifiée et activée).

Vous pouvez maintenant utiliser différentes stratégies sur différentes couches de cache, mais il y en a des « dangereuses ».

#### Invalidation basée sur un événement

![Invalidation pure basée sur un événement.](assets/chapter-3/event-based-invalidation.png)

*Invalidation pure basée sur un événement : invalidation du cache interne vers la couche externe*

<br> 

L’invalidation pure basée sur un événement est la plus facile à comprendre, à réaliser en théorie et la plus précise.

En termes simples, les caches sont invalidés un par un après la modification de l’objet.

Vous devez simplement garder une règle à l’esprit :

Invalidez toujours depuis le cache interne vers le cache externe. Si vous avez d’abord invalidé un cache externe, il se peut qu’il remette en cache le contenu obsolète d’un cache interne. Ne faites aucune supposition du moment où un cache est réactualisé ; veillez à ce que le cache soit vraiment réactualisé. Au mieux, en déclenchant l’invalidation du cache externe _après_ celle du cache interne.

Voilà la théorie. Mais en pratique il y a un certain nombre de pièges. Les événements doivent être distribués, potentiellement sur un réseau. En pratique, cela en fait le schéma d’invalidation le plus difficile à mettre en œuvre.

#### Auto dépannage

Avec l’invalidation basée sur un événement, vous devriez avoir un plan alternatif. Que se passe-t-il si un événement d’invalidation est manqué ? Une stratégie simple peut être d’invalider ou de purger après un certain temps. Vous avez peut-être manqué cet événement et vous avez maintenant diffusé du contenu obsolète. Mais vos objets ont également une durée de vie implicite de plusieurs heures (jours) uniquement. Finalement, le système se dépanne alors automatiquement.

#### Invalidation pure basée sur TTL

![Invalidation basée sur TTL non synchronisée.](assets/chapter-3/ttl-based-invalidation.png)

*Invalidation basée sur TTL non synchronisée.*

<br> 

C’est aussi un système assez courant. Vous empilez plusieurs couches de caches, chacune ayant le droit de servir un objet pendant un certain temps.

C’est facile à mettre en œuvre. Malheureusement, il est difficile de prédire la durée de vie effective d’une donnée.

![Cache externe prolongeant la durée de vie d’un objet interne.](assets/chapter-3/outer-cache.png)

*Cache externe prolongeant la durée de vie d’un objet interne.*

<br> 

Examinez l’illustration ci-dessus. Chaque couche de mise en cache ajoute une durée de vie (TTL) de 2 minutes. On pourrait alors penser que la durée de vie globale (TTL) doit être également de 2 minutes. Pas tout à fait. Si la couche externe récupère l’objet juste avant qu’il ne soit obsolète, la couche externe prolonge effectivement la durée de vie effective de l’objet. Dans ce cas, la durée de vie effective peut être comprise entre 2 et 4 minutes. Supposons que vous ayez convenu avec votre service commercial qu’une journée est acceptable et que vous disposez de quatre couches de caches. La durée de vie réelle de chaque couche ne doit pas dépasser six heures, ce qui augmente le taux d’absence de cache.

Ce n’est pas forcément un mauvais système. Il convient simplement d’en connaître les limites. Il s’agit d’une stratégie simple et efficace pour commencer. Ce n’est que si le trafic de votre site augmente que vous pouvez envisager une stratégie plus précise.

*Synchroniser l’heure d’invalidation en définissant une date spécifique*

#### Invalidation basée sur les dates d’expiration

La durée de vie effective est plus prévisible en définissant une date spécifique sur l’objet interne et en la propageant aux caches externes.

![Synchronisation des dates d’expiration.](assets/chapter-3/synchronize-expiration-dates.png)

*Synchronisation des dates d’expiration.*

<br> 

Cependant, tous les caches ne sont pas en mesure de propager les dates. Et la démarche peut devenir problématique lorsque le cache externe agrège deux objets internes avec des dates d’expiration différentes.

#### Mélanger des invalidations basées sur les événements et sur la durée de vie

![Mélange d’invalidations basées sur les événements et sur la durée de vie.](assets/chapter-3/mixing-event-ttl-strategies.png)

*Mélange d’invalidations basées sur les événements et sur la durée de vie.*

<br> 

Un autre système courant dans le monde AEM consiste à utiliser l’invalidation basée sur les événements dans les caches internes (par exemple, les caches en mémoire où les événements peuvent être traités en temps quasi réel) et les caches TTL à l’extérieur, où vous n’avez peut-être pas accès à l’invalidation explicite.

Dans le monde d’AEM, vous disposez d’un cache en mémoire pour les objets commerciaux et les fragments de HTML dans les systèmes de publication, qui est invalidé lorsque les ressources sous-jacentes changent et que vous propagez cet événement de modification au Dispatcher, qui fonctionne également selon les événements. En amont, vous auriez par exemple un réseau CDN basé sur la durée de vie.

Disposer d’une couche de mise en cache basée sur la durée de vie (courte) en amont de Dispatcher pourrait effectivement atténuer un pic qui se produirait généralement après une invalidation automatique.

#### Mélanger des invalidations basées sur la durée de vie et sur des événements

![Mélange d’invalidations basées sur la durée de vie et sur des événements.](assets/chapter-3/toxic.png)

*Dangereux : mélanger des invalidations basées sur la durée de vie et sur des événements*

<br> 

Cette combinaison est dangereuse. Ne placez jamais de cache basé sur un événement après une mise en cache basée sur la durée de vie ou sur l’expiration. Rappelez-vous cet effet de débordement que nous avons eu dans la stratégie « pure-TTL ». Le même effet peut être observé ici. Seulement, l’événement d’invalidation du cache externe qui s’est déjà produit ne se reproduira peut-être plus jamais, ce qui peut prolonger à l’infini la durée de vie de votre objet mis en cache.

![Invalidation basée sur TTL et basée sur les événements combinée : débordement infini.](assets/chapter-3/infinity.png)

*Invalidation basée sur TTL et basée sur les événements combinée : débordement infini.*

<br> 

## Mise en cache partielle et mise en cache en mémoire

Vous pouvez vous connecter à l’étape du processus de rendu pour ajouter des calques de mise en cache. De l’obtention d’objets de transfert de données distants ou la création d’objets métier locaux à la mise en cache du balisage rendu d’un seul composant. Nous consacrerons un prochain tutoriel aux mises en œuvre concrètes. Mais peut-être avez-vous déjà mis en œuvre vous-même quelques-unes de ces couches de mise en cache. Nous nous devons donc de présenter ici les principes de base, tout comme les pièges.

### Quelques avertissements

#### Respectez le contrôle d’accès.

Les techniques décrites ici sont assez puissantes et sont _indispensables_ dans la boîte à outils de chaque développeur ou développeuse AEM. Mais ne vous enflammez pas, utilisez-les sagement. Le fait de stocker un objet dans un cache et de le partager avec d’autres utilisateurs et utilisatrices dans des demandes de suivi revient à contourner le contrôle d’accès. Ce n’est en général pas un problème sur les sites web publics, mais cela le devient lorsqu’un utilisateur ou une utilisatrice doit se connecter avant d’y accéder.

Imaginez que vous stockiez le balisage HTML d’un menu principal de sites dans un cache en mémoire pour le partager entre différentes pages. Il s’agit en fait d’un exemple parfait pour stocker le HTML partiellement rendu, car la création d’une navigation est généralement coûteuse étant donné qu’elle nécessite de parcourir de nombreuses pages.

Vous ne partagez pas cette même structure de menus entre toutes les pages, mais également avec tous les utilisateurs et toutes les utilisatrices, ce qui la rend encore plus efficace. Mais attendez... il existe peut-être des éléments dans le menu qui ne sont réservés qu’à un certain groupe d’utilisateurs et d’utilisatrices. Dans ce cas, la mise en cache peut devenir un peu plus complexe.

#### Ne mettez en cache que les objets métier personnalisés.

Si vous ne devez retenir qu’un seul conseil, retenez le plus important :

>[!WARNING]
>
>Ne mettez en cache que les objets qui sont les vôtres, qui sont immuables, que vous avez construits vous-même, qui sont peu profonds et qui n’ont aucune référence sortante.

Qu’est-ce que cela signifie ?

1. Vous ne connaissez pas le cycle de vie prévu des objets des autres. Imaginez que vous obtenez une référence à un objet de requête, que vous décidez de mettre en cache. Désormais, la requête est terminée et le conteneur de servlets souhaite recycler cet objet pour la prochaine requête entrante. Dans ce cas, quelqu’un d’autre modifie le contenu dont vous pensiez avoir le contrôle exclusif. N’ignorez pas ce fait. Nous avons déjà vu quelque chose comme cela se produire dans un projet. Les clientes et clients voyaient des données d’autres clientes et clients au lieu des leurs.

2. Tant qu’un objet est référencé par une chaîne d’autres références, il ne peut pas être supprimé du tas. Si vous conservez dans votre cache un objet soi-disant petit qui fait référence, par exemple, à une représentation de 4 Mo d’une image, vous aurez une bonne chance de rencontrer des problèmes de fuite de mémoire. Les caches sont censés être basés sur des références faibles. Mais les références faibles ne fonctionnent pas comme prévu. Il s’agit du meilleur moyen de produire une fuite de mémoire et de se retrouver avec une erreur de mémoire insuffisante. De plus, vous ne savez pas quelle est la taille de la mémoire conservée des objets étrangers, n’est-ce-pas ?

3. En particulier dans Sling, vous pouvez adapter chaque objet (ou presque) l’un à l’autre. Imaginez que vous placez une ressource dans le cache. La requête suivante (avec des droits d’accès différents) récupère cette ressource et l’adapte à un ressourceResolver ou à une session pour accéder à d’autres ressources auxquelles il n’aurait pas accès.

4. Même si vous créez un mince « Wrapper » autour d’une ressource d’AEM, vous ne devez pas la mettre en cache, même si elle est la vôtre et qu’elle est immuable. L’objet encapsulé serait une référence (que nous interdisions auparavant) et si nous regardons de plus près, cela crée fondamentalement les mêmes problèmes que ceux décrits dans la section précédente.

5. Si vous souhaitez faire une mise en cache, créez vos propres objets en copiant des données primitives dans vos propres objets peu profonds. Vous pouvez lier vos propres objets en fonction des références ; vous pouvez par exemple mettre en cache une arborescence d’objets. Tout cela est très bien, mais ne mettez en cache que les objets que vous venez de créer dans la même requête, et aucun objet qui a été demandé à un autre endroit (même s’il s’agit de l’espace de noms de « votre » objet). La _copie d’objets_ est la clé. Veillez également à purger la structure entière des objets liés en même temps et à éviter les références entrantes et sortantes de votre structure.

6. Veillez également à rendre vos objets non modifiables. Propriétés privées uniquement et sans méthodes setter.

Il y a beaucoup de règles, mais ça vaut la peine de les suivre. Et ce, même si vous avez toute l’expérience et l’intelligence du monde et que vous contrôlez tout. Votre jeune collègue qui travaille sur votre projet vient d’obtenir son diplôme de l’université. Il ne connaît pas tous ces écueils. S’il n’y a pas d’écueils, il n’y a rien à éviter. Simplicité et compréhension sont les maîtres mots.

### Outils et bibliothèques

Cette série porte sur la compréhension des concepts et vous permet de créer une architecture adaptée à votre cas d’utilisation.

Nous ne faisons la promotion d’aucun outil en particulier. Mais nous vous apportons des indices pour les évaluer. Par exemple, AEM a un cache intégré simple avec un TTL fixe depuis la version 6.0. L’utiliserez-vous ? Probablement pas sur une instance de publication où un cache basé sur un événement suit dans la chaîne (indice : le Dispatcher). Mais cela pourrait être un choix acceptable dans une instance de création. Il existe également un cache HTTP par Adobe ACS Commons qui peut être intéressant à envisager.

Ou vous pouvez créer le vôtre en fonction d’un framework de mise en cache mature comme [Ehcache](https://www.ehcache.org). Il peut être utilisé pour mettre en cache des objets Java et des balises générées (objets `String`).

Dans certains cas simples, vous pouvez également utiliser des cartes de hachage simultanées, dont vous verrez rapidement les limites, soit dans l’outil, soit dans vos compétences. La simultanéité est aussi difficile à maîtriser que le nommage et la mise en cache.

#### Références

* [Cache HTTP ACS Commons](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Framework de mise en cache Ehcache](https://www.ehcache.org)

### Termes de base

Nous n’irons pas trop loin dans la théorie de la mise en cache ici, mais nous sentons l’obligation de parler de quelques mots à la mode pour que vous commenciez sur de bonnes bases.

#### Expulsion du cache

Nous avons beaucoup parlé d’invalidation et de purge. L’_expulsion du cache_ est liée à ces termes : après l’expulsion d’une entrée, elle n’est plus disponible. Mais l’expulsion ne se produit pas lorsqu’une entrée est obsolète, mais lorsque le cache est plein. Les éléments plus récents ou « plus importants » poussent ceux plus anciens ou moins importants hors du cache. Les entrées que vous devrez sacrifier sont une décision au cas par cas. Vous pouvez vouloir expulser les plus anciennes ou celles qui ont été utilisées très rarement ou il y a longtemps.

#### Mise en cache préventive

La mise en cache préventive consiste à recréer l’entrée avec du contenu nouveau au moment où elle est invalidée ou considérée comme obsolète. Bien sûr, vous ne l’utiliseriez qu’avec quelques ressources dont vous avez la certitude qu’elles sont fréquemment consultées et immédiatement. Sinon, vous gaspillez des ressources pour créer des entrées de cache qui ne seront peut-être jamais demandées. En créant des entrées de cache de manière préventive, vous pouvez réduire la latence de la première requête à une ressource après l’invalidation du cache.

#### Préchauffage du cache

Le préchauffage du cache est étroitement lié à la mise en cache préventive. Bien que vous n’utilisiez pas ce terme pour désigner un système actif. Et le temps y est moins limité. Vous ne remettez pas en cache immédiatement après l’invalidation, mais vous remplissez le cache progressivement lorsque le temps le permet.

Par exemple, vous retirez un composant de Publication/Dispatcher de la répartition de charge pour le mettre à jour. Avant de le réintégrer, vous analysez automatiquement les pages les plus fréquemment consultées pour les placer à nouveau dans le cache. Lorsque le cache est « chaud » et rempli de manière adéquate, vous réintégrez le composant dans la répartition de charge.

Ou peut-être que vous réintégrez le composant tout de suite, mais vous réduisez le trafic jusqu’à ce composant afin qu’il ait une chance de préchauffer ses caches par usage régulier.

Ou peut-être vous faut-il également mettre en cache certaines pages moins fréquemment consultées lorsque votre système est inactif pour diminuer la latence lorsqu’elles sont enfin consultées par des requêtes réelles.

#### Identité d’objet du cache, payload, dépendance d’invalidation et TTL

En règle générale, un objet mis en cache ou une « entrée » possède cinq propriétés principales.

#### Clé

Il s’agit de l’identité, la propriété par laquelle vous identifiez l’objet. pour récupérer sa payload ou la purger du cache. Le Dispatcher utilise, par exemple, l’URL d’une page comme clé. Notez que le Dispatcher n’utilise pas les chemins d’accès aux pages. Cela ne suffit pas à distinguer les différents rendus. D’autres caches peuvent utiliser des clés différentes. Nous verrons quelques exemples plus tard.

#### Valeur / Payload

C’est le trésor de l’objet, les données que vous voulez récupérer. Dans le cas du Dispatcher, il s’agit du contenu des fichiers. Mais il peut également s’agir d’une arborescence d’objets Java.

#### TTL

Nous avons déjà traité le TTL. Il s’agit du temps au-delà duquel une entrée est considérée comme obsolète et ne doit plus être diffusée.

#### Dépendance

Il s’agit d’une invalidation basée sur un événement. De quelles données originales cet objet dépend-il ? Dans la partie 1, nous avons déjà indiqué qu’un suivi véritable et précis des dépendances est trop complexe. Mais grâce à votre connaissance du système, vous pouvez déterminer les dépendances à l’aide d’un modèle plus simple. Nous invalidons suffisamment d’objets pour purger le contenu obsolète... et peut-être, par inadvertance, plus que nécessaire. Pourtant, nous essayons de rester en deçà du « tout purger ».

Les objets qui dépendent des autres varient en fonction de chaque application. Plus loin dans l’article, vous pourrez consulter quelques exemples de mise en œuvre d’une stratégie de dépendance.

### Mise en cache de fragments HTML

![Réutilisation d’un fragment rendu sur différentes pages](assets/chapter-3/re-using-rendered-fragment.png).

*Réutilisation d’un fragment rendu sur différentes pages*.

<br> 

La mise en cache de fragments HTML est d’une puissance redoutable. L’idée est de mettre en cache le balisage HTML généré par un composant dans un cache en mémoire. Vous vous demandez certainement dans quel but ? Car, de toute façon, le balisage de la page entière est mis en cache dans le Dispatcher, y compris le balisage de ce composant. Certes. Mais vous devez effectuer cette opération pour chaque page. Vous ne partagez pas ce balisage entre les pages.

Imaginez que vous générez une navigation en haut de chaque page. Le balisage est identique sur chaque page. Mais vous le restituez encore et encore pour chaque page, qui n’est pas dans le Dispatcher. Et n’oubliez pas : après l’invalidation automatique, toutes les pages doivent être rendues à nouveau. En somme, vous exécutez le même code avec les mêmes résultats des centaines de fois.

De notre expérience, le rendu d’une navigation supérieure imbriquée est une tâche très coûteuse. En règle générale, vous parcourez une bonne partie de l’arborescence du document pour générer les éléments de navigation. Même si vous n’avez besoin que du titre de navigation et de l’URL, les pages doivent être chargées en mémoire. Des ressources précieuses doivent alors être utilisées. Encore et encore.

Mais le composant est partagé entre plusieurs pages. Le partage implique l’utilisation d’un cache. Par conséquent, vous devez vérifier si le composant de navigation a déjà été rendu et mis en cache et, au lieu d’effectuer un nouveau rendu, il vous suffit d’émettre la valeur de cache.

Remarquez les deux subtilités suivantes :

1. Vous mettez en cache une chaîne Java. Une chaîne ne comporte aucune référence sortante et est non modifiable. Donc, compte tenu des avertissements ci-dessus, il s’agit d’une méthode très sûre.

2. L’invalidation est aussi très facile. Chaque fois qu’une modification est apportée à votre site web, vous devez invalider cette entrée du cache. Cette opération n’est guère contraignante, car elle ne doit être effectuée qu’une seule fois pour les centaines de pages du site.

Vos serveurs de publication vous doivent une fière chandelle.

### Mettre en œuvre les caches de fragments

#### Balises personnalisées

Autrefois, lorsque vous utilisiez JSP comme moteur de modèle, il était assez courant d’utiliser une balise JSP personnalisée encapsulant le code de rendu des composants.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

La balise personnalisée capturait son corps et l’écrivait dans le cache ou empêchait son exécution et affichait à la place la payload de l’entrée du cache.

La « clé » était le chemin d’accès aux composants sur la page d’accueil. Nous n’utilisons pas le chemin du composant sur la page active, car cela créerait une entrée de cache par page, ce qui irait à l’encontre de notre intention de partager ce composant. Nous n’utilisons pas uniquement le chemin relatif des composants (`jcr:conten/mainnavigation`), car cela nous empêcherait d’utiliser différents composants de navigation sur différents sites.

Le « Cache » est un indicateur de l’endroit où stocker l’entrée. Vous disposez généralement de plusieurs caches dans lesquels vous stockez des éléments. Chacun d’entre eux peut se comporter un peu différemment. Il est donc bon de différencier ce qui est stocké, même si finalement, ce ne sont que des chaînes.

« Dépendance » : c’est de cela que dépend l’entrée du cache. Le cache de « navigation principale » peut comporter une règle : en cas de modification sous le nœud « dépendance », l’entrée correspondante doit être purgée. Ainsi, votre implémentation du cache doit s’enregistrer en tant qu’écouteur d’événement dans le référentiel pour connaître les modifications, puis appliquer les règles spécifiques au cache pour déterminer ce qui doit être invalidé.

Ceci n’est qu’un exemple. Vous pouvez également choisir d’avoir une arborescence de caches. Lorsque le premier niveau est utilisé pour séparer les sites (ou les clientes et clients) et le second niveau est déployé dans des types de contenu (par exemple, « navigation principale »), cela peut vous empêcher d’ajouter le chemin des pages d’accueil comme dans l’exemple ci-dessus.

D’ailleurs, vous pouvez également utiliser cette approche avec des composants HTL plus modernes. Vous obtiendriez alors un Wrapper JSP autour de votre script HTL.

#### Filtres de composant

Mais dans une approche HTL pure, il vaut mieux créer le cache de fragments avec un filtre de composant Sling. Nous n’avons pas encore rencontré ce problème dans la pratique, mais c’est l’approche que nous adopterions.

#### Sling Dynamic Include

Le cache de fragments est utilisé si vous avez quelque chose de constant (la navigation) dans le contexte d’un environnement changeant (différentes pages).

Mais vous pouvez également avoir le contraire, un contexte relativement constant (une page qui change rarement) et certains fragments qui changent constamment sur cette page (par exemple, une horloge en direct).

Dans ce cas, vous pouvez donner une chance aux [inclusions dynamiques Sling](https://sling.apache.org/documentation/bundles/dynamic-includes.html). Il s’agit essentiellement d’un filtre de composant qui entoure le composant dynamique et, au lieu de générer le rendu du composant dans la page, il crée une référence. Cette référence peut être un appel Ajax, de sorte que le composant soit inclus par le navigateur et que la page environnante puisse être mise en cache de manière statique. Ou Sling Dynamic Include (SDI) peut générer une directive SSI (Server Side Include). Cette directive serait exécutée dans le serveur Apache. Vous pouvez même utiliser les directives ESI (Edge Side Include) si vous utilisez un cache Varnish ou un réseau CDN qui prend en charge les scripts ESI.

![Diagramme de séquence d’une requête à l’aide de Sling Dynamic Include.](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagramme de séquence d’une requête à l’aide de Sling Dynamic Include.*

<br> 

La documentation sur les SDI indique que vous devez désactiver la mise en cache des URL se terminant par « *.nocache.html », ce qui est logique, car vous traitez des composants dynamiques.

Vous pouvez voir une autre option pour utiliser les SDI : si vous ne désactivez _pas_ le cache du Dispatcher pour les inclusions, le Dispatcher agit comme un cache de fragments similaire à celui que nous avons décrit dans le dernier chapitre : les pages et les fragments de composant sont mis en cache de manière égale et indépendante dans le Dispatcher et assemblés par le script SSI dans le serveur Apache lorsque la page est demandée. Cela vous permet de mettre en œuvre des composants partagés comme la navigation principale (à condition que vous utilisiez toujours la même URL de composant).

Cela devrait marcher, en théorie. Mais...

Nous vous conseillons de ne pas faire cela : vous perdriez la capacité de contourner le cache pour les composants dynamiques réels. Les SDI sont configurées globalement et les modifications que vous apporteriez à votre « cache de fragments de fortune » s’appliqueraient également aux composants dynamiques.

Nous vous conseillons d’étudier attentivement la documentation sur les SDI. Il existe d’autres restrictions, mais les SDI constituent un outil précieux dans certains cas.

#### Références

* [docs.oracle.com - Comment écrire des balises JSP personnalisées](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Créer et utiliser des filtres de composants](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configurer les Sling Dynamic Includes dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-sling-dynamic-include.html?lang=fr)


#### Mise en cache des modèles

![Mise en cache basée sur un modèle : un objet métier avec deux rendus différents.](assets/chapter-3/model-based-caching.png)

*Mise en cache basée sur un modèle : un objet métier avec deux rendus différents.*

<br> 

Reprenons l’exemple avec la navigation. Nous supposions que chaque page nécessiterait le même balisage de navigation.

Mais ce n’est peut-être pas le cas. Vous pouvez souhaiter effectuer le rendu de différentes balises de l’élément dans la navigation qui représente la _page actuelle_.

```
Travel Destinations

<ul class="maninnav">
  <li class="currentPage">Travel Destinations
    <ul>
      <li>Finland
      <li>Canada
      <li>Norway
    </ul>
  <li>News
  <li>About us
<ul>
```

```
News

<ul class="maninnav">
  <li>Travel Destinations
  <li class="currentPage">News
    <ul>
      <li>Winter is coming>
      <li>Calm down in the wild
    </ul>
  <li>About us
<is
```

Il s’agit de deux rendus complètement différents. Pourtant, l’_objet métier_, c’est-à-dire l’arborescence de navigation complète, est le même.  L’_objet métier_ serait ici un graphe d’objets représentant les nœuds de l’arborescence. Ce graphe peut facilement être stocké dans un cache en mémoire. N’oubliez pas, cependant, que ce graphe ne doit pas contenir d’objet ni référencer un objet que vous n’avez pas créé vous-même, en particulier les nœuds JCR.

#### Mise en cache dans le navigateur

Nous avons déjà abordé l’importance de la mise en cache dans le navigateur, et il existe de nombreux tutoriels de qualité sur le sujet. En fin de compte, pour le navigateur, le Dispatcher est simplement un serveur web qui suit le protocole HTTP.

Cependant, malgré la théorie, nous avons rassemblé des connaissances que nous n’avons trouvées nulle part ailleurs et que nous voulons partager.

Essentiellement, la mise en cache du navigateur peut être utilisée de deux manières différentes,

1. Le navigateur comporte une ressource mise en cache dont il connaît la date d’expiration exacte. Dans ce cas, il ne redemande pas la ressource.

2. Le navigateur dispose d’une ressource, mais il ne sait pas si elle est toujours valide. Dans ce cas, il demande au serveur web (le Dispatcher dans notre cas) : « s’il vous plaît, donnez-moi la ressource si elle a été modifiée depuis sa dernière diffusion ». Si elle n’a pas changée, le serveur répond par « 304 - not changed » et seules les métadonnées sont transmises.

#### Débogage

Si vous optimisez les paramètres de Dispatcher pour la mise en cache du navigateur, il est extrêmement utile d’utiliser un serveur proxy de bureau entre votre navigateur et le serveur web. Nous préférons « Charles Web Debugging Proxy » de Karl von Randow.

Avec Charles, vous pouvez lire les requêtes et les réponses, qui sont transmises au serveur et depuis le serveur. Vous pouvez également en apprendre beaucoup sur le protocole HTTP. Les navigateurs modernes offrent également des fonctionnalités de débogage, mais les fonctionnalités d’un proxy de bureau sont inégalées. Vous pouvez manipuler les données transférées, ralentir la transmission, relayer les requêtes uniques et bien plus encore. De plus, l’interface utilisateur est clairement organisée et assez complète.

Le test le plus élémentaire consiste à utiliser le site web comme une personne normale (avec le proxy entre les deux) et à vérifier si le nombre de requêtes statiques (vers /etc/...) diminue au fil du temps, car elles doivent se trouver dans le cache et ne plus être demandées.

Nous avons vu qu’un proxy peut donner un aperçu plus clair, car la requête mise en cache n’apparaît pas dans le journal, alors que certains débogueurs intégrés au navigateur affichent toujours ces requêtes avec « 0 ms » ou « from disk ». Ce qui est acceptable et exact, mais pourrait troubler votre jugement.

Vous pouvez ensuite descendre dans la hiérarchie et vérifier les en-têtes des fichiers transférés pour voir, par exemple, si les en-têtes HTTP « Expire » sont corrects. Vous pouvez lire à nouveau les requêtes avec des en-têtes if-modified-since définis pour voir si le serveur répond correctement avec un code de réponse 304 ou 200. Vous pouvez observer la durée des appels asynchrones et tester vos hypothèses de sécurité dans une certaine mesure. Vous vous souvenez que nous vous avons dit de ne pas accepter tous les sélecteurs qui ne sont pas explicitement attendus ? Ici, vous pouvez jouer avec l’URL et les paramètres et voir si votre application se comporte correctement.

Il y a une seule chose que nous vous demandons de ne pas faire lorsque vous déboguez votre cache :

Ne rechargez pas les pages dans le navigateur.

Un « rechargement de navigateur », un _simple-reload_ ainsi qu’un _forced-reload_ (« _shift-reload_ ») est différent d’une requête de page normale. Une requête de rechargement simple définit un en-tête.

```
Cache-Control: max-age=0
```

En outre, un « shift-reload » (maintenir la touche Maj enfoncée tout en cliquant sur le bouton de rechargement) définit généralement un en-tête de requête.

```
Cache-Control: no-cache
```

Les deux en-têtes ont des effets similaires mais légèrement différents, mais surtout, ils diffèrent complètement d’une requête normale lorsque vous ouvrez une URL à partir de l’emplacement d’URL ou en utilisant des liens sur le site. La navigation normale ne définit pas les en-têtes Cache-Control, mais probablement un en-tête if-modified-since.

Ainsi, si vous souhaitez déboguer le comportement de navigation normal, vous devez _naviguer normalement_. L’utilisation du bouton Recharger de votre navigateur est le meilleur moyen de ne pas afficher les erreurs de configuration du cache dans votre configuration.

Utilisez votre proxy Charles pour découvrir de quoi nous parlons. Oui, et pendant qu’il est ouvert, vous pouvez lire les requêtes ici. Il n’est pas nécessaire de recharger à partir du navigateur.

## Test de performances

L’utilisation d’un proxy vous permet d’avoir une idée du comportement de la durée de vos pages. Bien entendu, il ne s’agit pas d’un test de performance.  Un test de performance nécessite un certain nombre de clienets ou clients qui demandent vos pages en parallèle.

Une erreur courante, comme nous l’avons vu trop souvent, est que le test de performance inclut uniquement un très petit nombre de pages et que ces pages sont diffusées uniquement à partir du cache de Dispatcher.

Si vous faites la promotion de votre application sur le système en ligne, la charge est complètement différente de celle que vous avez testée.

Sur le système en ligne, le modèle d’accès n’est pas le petit nombre de pages uniformément réparties dont vous disposez dans les tests (page d’accueil et quelques pages de contenu). Le nombre de pages est beaucoup plus important et les demandes sont très inégalement réparties. Et, bien sûr, les pages actives ne peuvent pas être diffusées à 100 % à partir du cache : il existe des demandes d’invalidation provenant du système de publication qui invalident automatiquement une grande partie de vos précieuses ressources.

Ah oui, et quand vous allez recréer votre cache de Dispatcher, vous allez découvrir que le système de publication se comporte également très différemment, selon que vous demandez seulement une poignée de pages ou un nombre plus élevé. Même si toutes les pages sont d’une complexité similaire, leur nombre joue un rôle important. Vous vous rappelez ce que nous avons dit sur la mise en cache en chaîne ? Si vous demandez toujours le même petit nombre de pages, il y a de bonnes chances que les blocs correspondants avec les données brutes se trouvent dans le cache du disque dur ou que les blocs soient mis en cache par le système d’exploitation. Il est également possible que le référentiel ait mis en cache le segment correspondant dans sa mémoire principale. Le rendu est donc beaucoup plus rapide que lorsque d’autres pages s’expulsent de temps à autre de divers caches.

La mise en cache est difficile, de même que le test d’un système qui repose sur la mise en cache. Alors, que pouvez-vous faire pour avoir un reflet plus précis de la réalité ?

Nous pensons que vous devriez effectuer plusieurs tests et que vous devriez fournir plusieurs index de performance pour mesurer la qualité de votre solution.

Si vous disposez déjà d’un site web, mesurez le nombre de requêtes et leur mode de répartition. Essayez de modéliser un test qui utilise une répartition similaire des requêtes. Ajouter un peu d’aléatoire ne peut pas faire mal. Il n’est pas nécessaire de simuler un navigateur qui chargerait des ressources statiques telles que JS et CSS, car elles n’ont pas vraiment d’importance. Elles sont mises en cache dans le navigateur ou dans Dispatcher ultérieurement et n’augmentent pas la charge de manière significative. Mais les images référencées ont leur importance. Recherchez également leur réparition dans d’anciens fichiers journaux et modélisez un modèle de requête similaire.

Effectuez maintenant un test avec votre Dispatcher sans aucune mise en cache. Il s’agit de votre pire scénario. Découvrez à quel niveau de charge votre système devient instable dans ces pires conditions. Vous pouvez également empirer les choses en supprimant quelques phases Dispatcher/de publication si vous le souhaitez.

Exécutez ensuite le même test avec tous les paramètres de cache requis activés. Augmentez lentement vos requêtes parallèles pour préchauffer le cache et voir ce que votre système peut supporter dans ces conditions optimales.

Un scénario de cas moyen consiste à exécuter le test avec Dispatcher activé, mais également avec certaines invalidations. Vous pouvez simuler cela en touchant les fichiers stat avec une tâche cron ou en envoyant les demandes d’invalidation à intervalles irréguliers au Dispatcher. N’oubliez pas également de purger de temps à autre certaines ressources qui ne sont pas automatiquement invalidées.

Vous pouvez ajuster ce scénario en augmentant les demandes d’invalidation et en augmentant la charge.

C’est un peu plus complexe qu’un simple test de charge linéaire, mais cela donne beaucoup plus de confiance dans votre solution.

Vous pourriez hésiter à faire ces efforts. Mais effectuez au moins un test du pire cas sur le système de publication avec un grand nombre de pages (réparties de manière égale) pour voir les limites du système. Assurez-vous que vous interprétez correctement le nombre des scénarios les plus probants et que vous configurez vos systèmes avec suffisamment de marge de manœuvre.
