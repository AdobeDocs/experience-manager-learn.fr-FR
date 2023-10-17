---
title: « Chapitre 2 - Infrastructure de Dispatcher »
description: Comprenez la topologie de publication et de Dispatcher. Découvrez les topologies et configurations les plus courantes.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
exl-id: a25b6f74-3686-40a9-a148-4dcafeda032f
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: ht
source-wordcount: '1864'
ht-degree: 100%

---

# Chapitre 2 - Infrastructure

## Configurer une infrastructure de mise en cache

Nous avons présenté la topologie de base d’un système de publication et d’un Dispatcher dans le chapitre 1 de cette série. Un ensemble de serveurs de publication et de Dispatcher peut être configuré en de nombreuses variantes : selon la charge attendue, la topologie de votre ou vos centres de données et les propriétés de basculement souhaitées.

Nous allons esquisser les topologies les plus courantes et décrire leurs avantages et leurs limites. La liste n’est, bien entendu, pas exhaustive. La seule limite est votre imagination.

### Configuration « héritée »

À l’époque, le nombre de visiteurs et visiteuses potentiels était faible, le matériel coûteux et les serveurs web n’étaient pas considérés comme aussi essentiels qu’aujourd’hui. Une configuration courante consistait à faire en sorte qu’un Dispatcher serve de répartition de charge et de cache devant deux systèmes de publication ou plus. Le serveur Apache au cœur de Dispatcher était très stable et, dans la plupart des paramètres, capable de répondre à un bon nombre de requêtes.

![Configuration de Dispatcher « héritée » : peu courante aujourd’hui.](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuration de Dispatcher « héritée » : peu courante aujourd’hui.*

<br> 

C’est de là que le Dispatcher a hérité son nom : il répartissait (« dispatch », en anglais) essentiellement des requêtes. Cette configuration n’est plus très courante, car elle ne peut plus répondre aux exigences de performances et de stabilité plus élevées requises aujourd’hui.

### Configuration multiphase

De nos jours, une topologie légèrement différente est plus courante. Une topologie multiphase comporterait un Dispatcher par serveur de publication. Une répartition de charge dédiée (matérielle) se trouve devant l’infrastructure AEM qui répartit les requêtes à ces deux (ou plus) phases :

![Configuration moderne du Dispatcher « standard » : facile à gérer et à maintenir.](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuration moderne du Dispatcher « standard » : facile à gérer et à maintenir.*

<br> 

Voici les raisons qui justifient ce type de configuration :

1. En moyenne, les sites web diffusent beaucoup plus de trafic que par le passé. Il est donc nécessaire d’augmenter « l’infrastructure Apache ».

2. La configuration « héritée » ne fournissait pas de redondance au niveau du Dispatcher. Si le serveur Apache plantait, l’ensemble du site web était inaccessible.

3. Les serveurs Apache sont peu coûteux. Ils sont basés sur l’open source et,si vous disposez d’un centre de données virtuel, ils peuvent être configurés très rapidement.

4. Cette configuration permet d’obtenir facilement un scénario de mise à jour « variable » ou « échelonné ». Il vous suffit d’arrêter Dispatcher 1 lors de l’installation d’un nouveau package de logiciels sur le serveur de publication 1. Une fois l’installation terminée et que vous avez suffisamment testé le serveur de publication 1 sur le réseau interne, nettoyez le cache sur Dispatcher 1 et redémarrez-le tout en arrêtant Dispatcher 2 pour maintenance du serveur de publication 2.

5. L’invalidation du cache devient très facile et déterministe dans cette configuration. Comme un seul système de publication est connecté à un seul Dispatcher, il n’y a qu’un seul Dispatcher à invalider. L’ordre et le timing de l’invalidation sont sans importance.

### Configuration de « mise à l’échelle »

Comme les serveurs Apache sont peux coûteux et faciles à configurer, pourquoi ne pas augmenter un peu plus leur mise à l’échelle. Pourquoi ne pas avoir deux ou plusieurs Dispatchers devant chaque serveur de publication ?

![Configuration de « mise à l’échelle » : elle comporte des zones d’application, mais aussi des limites et des mises en garde.](assets/chapter-2/scale-out-setup.png)

*Configuration de « mise à l’échelle » : elle comporte des zones d’application, mais aussi des limites et des mises en garde.*

<br> 

Vous pouvez absolument procéder ainsi. Et il y a beaucoup de scénarios d’application valides pour cette configuration. Mais il y a aussi des limites et des complexités que vous devriez prendre en compte.

#### Invalidation

Chaque système de publication est connecté à une multitude de Dispatchers. Chacun d’eux doit être invalidé lorsque le contenu a été modifié.

#### Maintenance

Il va sans dire que la configuration initiale des systèmes Dispatcher et de publication est un peu plus complexe. Mais gardez également à l’esprit qu’une version de « déploiement » implique plus de travail. Les systèmes AEM peuvent et doivent être mis à jour lors de l’exécution. Mais il vaut mieux ne pas le faire pendant qu’ils traitent activement des demandes. En règle générale, il est préférable de mettre à jour uniquement une partie des systèmes de publication, tandis que les autres servent toujours activement le trafic, puis, après les tests, vous passer à l’autre partie. Si vous avez de la chance et que vous pouvez accéder au répartiteur de charge dans votre processus de déploiement, vous pouvez désactiver le routage vers les serveurs en cours de maintenance. Si vous utilisez un répartiteur de charge partagé sans accès direct, il vaut mieux arrêter les Dispatchers du système de publication que vous souhaitez mettre à jour. Plus il y en a, plus vous devrez en arrêter. S’il y a un grand nombre de Dispatchers et que vous planifiez des mises à jour fréquentes, l’automatisation est recommandée. Si vous n’avez pas d’outils d’automatisation, la mise à l’échelle est de toute façon une mauvaise idée.

Dans un projet précédent, nous avons utilisé une autre astuce pour supprimer un système de publication de la répartition de charge sans avoir un accès direct au répartiteur de charge.

En général, la répartition de charge envoie un « ping » à une page spécifique pour voir si le serveur est en cours d’exécution. L’option habituelle consiste à envoyer un ping à la page d’accueil. Mais si vous voulez utiliser le ping pour signaler à la répartition de charge de ne pas équilibrer le trafic, une meilleure option s’offre à vous. Vous créez un modèle ou un servlet dédié qui peut être configuré pour répondre avec `"up"` ou `"down"` (dans le corps ou en tant que code de réponse HTTP). La réponse de cette page ne doit bien sûr pas être mise en cache dans le Dispatcher. Elle est donc toujours fraîchement récupérée du système de publication. Maintenant, si vous configurez la répartition de charge pour vérifier ce modèle ou servlet, vous pouvez facilement laisser le système de publication « faire semblant » qu’il est en panne. Il ne fait pas partie de la répartition de charge et peut être mis à jour.

#### Distribution mondiale

La « distribution mondiale » est une configuration de « mise à l’échelle » où vous avez plusieurs Dispatchers devant chaque système de publication, désormais répartis dans le monde entier pour être plus proches de la clientèle et offrir de meilleures performances. Bien sûr, dans ce scénario, vous n’avez pas de répartition de charge centrale mais un schéma de répartition de charge basé sur DNS et géo-IP.

>[!NOTE]
>
>En fait, vous créez une sorte de réseau de diffusion de contenu (réseau CDN) avec cette approche. Vous devriez donc envisager d’acheter une solution de réseau CDN standard plutôt que de la créer vous-même. Créer et maintenir un réseau CDN n’est pas une tâche facile.

#### Mise à l’échelle horizontale

Même dans un centre de données local, une topologie de « mise à l’échelle » avec plusieurs Dispatchers devant chaque système de publication présente certains avantages. Si vous constatez des goulots d’étranglement sur les performances des serveurs Apache en raison d’un trafic élevé (et d’un fort taux d’accès au cache) et que vous ne pouvez plus augmenter le matériel (en ajoutant des processeurs, de la mémoire vive et des disques plus rapides), vous pouvez améliorer les performances en ajoutant des Dispatchers. On parle alors de « mise à l’échelle horizontale ». Toutefois, cela comporte des limites, en particulier lorsque vous invalidez fréquemment le trafic. L’effet est présenté dans la section suivante.

#### Limites de la topologie de mise à l’échelle

L’ajout de serveurs proxy doit normalement améliorer les performances. Cependant, il existe des scénarios où l’ajout de serveurs peut en fait réduire les performances. Comment ? Supposons que vous ayez un portail d’actualités, sur lequel vous ajoutez de nouveaux articles et de nouvelles pages chaque minute. Un Dispatcher invalide par « invalidation automatique » : chaque fois qu’une page est publiée, toutes les pages du cache sur le même site sont invalidées. Cette fonctionnalité est utile, nous l’avons décrite dans le [chapitre 1](chapter-1.md) de cette série. Mais cela signifie également que lorsque vous effectuez de fréquentes modifications sur votre site web, vous invalidez assez souvent le cache. Si vous ne disposez que d’un seul Dispatcher par instance de publication, la première personne qui demande une page déclenche une nouvelle mise en cache de cette page. La deuxième personne obtient la version mise en cache.

Si vous disposez de deux Dispatchers, la deuxième personne a 50 % de chances que la page ne soit pas mise en cache, et elle subira une latence plus importante lors du rendu de la page. Le fait d’avoir encore plus de Dispatchers par système de publication rend les choses encore plus difficiles. Ce qui se passe, c’est que le serveur de publication reçoit plus de charge, car il doit effectuer le rendu de la page pour chaque Dispatcher séparément.

![Diminution des performances dans un scénario de mise à l’échelle avec vidages fréquents du cache.](assets/chapter-2/decreased-performance.png)

*Diminution des performances dans un scénario de mise à l’échelle avec vidages fréquents du cache.*

<br> 

#### Atténuer les problèmes de mise à l’échelle excessive

Vous pouvez envisager d’utiliser un stockage partagé central pour tous les Dispatchers ou de synchroniser les systèmes de fichiers des serveurs Apache afin d’atténuer les problèmes. Nous n’avons qu’une expérience limitée dans ce domaine, mais ayez conscience que cela ajoute à la complexité du système et peut introduire une toute nouvelle classe d’erreurs.

Nous avons fait quelques expériences avec NFS, mais NFS présente d’énormes problèmes de performances en raison du verrouillage du contenu. Cela a en fait diminué la performance globale.

**Conclusion** : partager un système de fichiers commun entre plusieurs Dispatchers n’est PAS une approche recommandée.

Si vous rencontrez des problèmes de performances, redimensionnez les instances de publication et de Dispatcher de manière égale afin d’éviter un pic de charge sur les instances de publication. Il n’existe pas de règle d’or concernant le ratio publication/Dispatcher ; il dépend fortement de la distribution des requêtes et de la fréquence des publications et des invalidations du cache.

Si la latence ressentie par un visiteur ou une visiteuse vous préoccupe également, envisagez d’utiliser un réseau de diffusion de contenu, la récupération du cache, le préchauffage préemptif du cache et la définition d’un délai de grâce tel que décrit au [Chapitre 1](chapter-1.md) de cette série ou référez-vous à des idées avancées de la [Partie 3](chapter-3.md).

### Configuration « Cross Connected » (connexion croisée)

Une autre configuration que nous avons vue de temps à autre est la configuration dite « croisée » : les instances de publication n’ont pas de Dispatchers dédiés, mais tous les Dispatchers sont connectés à tous les systèmes de publication.

![Topologie croisée : redondance accrue et plus de complexité](assets/chapter-2/cross-connected-setup.png).

<br> 

*Topologie interconnectée : redondance accrue et plus de complexité.*

À première vue, cela donne une plus grande redondance pour un budget relativement modeste. Lorsque l’un des serveurs Apache est hors service, deux systèmes de publication peuvent toujours effectuer le rendu. En outre, si l’un des systèmes de publication se bloque, deux Dispatchers servent toujours la charge mise en cache.

Mais cela a un prix.

Tout d’abord, retirer une phase pour la maintenance est un processus assez lourd. En fait, c’est pour cela que ce projet a été conçu : pour être plus résilient et rester en activité par tous les moyens possibles. Nous avons vu des plans de maintenance complexes sur la façon de gérer tout cela. Reconfigurez Dispatcher 2 en premier, en supprimant la connexion croisée. Redémarrage de Dispatcher 2. Arrêt de Dispatcher 1, mise à jour de la publication 1, etc. Vous devriez y réfléchir à deux fois si ce processus montait à plus de deux phases. Vous en arriverez à la conclusion que cela ne fait qu’augmenter la complexité et les coûts et qu’il s’agit d’une source formidable d’erreur humaine. Il serait donc préférable d’automatiser tout cela. Donc faites-y attention si vous avez bien les ressources humaines vous permettant d’inclure cette tâche d’automatisation dans le planning de votre projet. Vous économiserez peut-être des coûts matériels, mais vous dépenserez aussi peut-être deux fois plus en personnel informatique.

Deuxièmement, il se peut qu’une application utilisateur s’exécute sur l’AEM qui requiert une connexion. Vous utilisez des affinités de sessions pour vous assurer qu’un utilisateur ou une utilisatrice est toujours servi(e) à partir de la même instance AEM afin que vous puissiez conserver le statut de session sur cette instance. Avec cette configuration croisée, vous devez vous assurer que les affinités de sessions fonctionnent correctement sur la répartition de charge et sur les Dispatchers. Pas impossible, mais vous devez en avoir conscience et ajouter des heures de configuration et de test supplémentaires, ce qui, encore une fois, peut vous faire économiser du matériel.

### Conclusion

Nous vous déconseillons d’utiliser ce schéma de connexion croisée comme option par défaut. Cependant, si vous décidez de l’utiliser, vous devrez soigneusement en évaluer les risques et les coûts cachés et planifier d’inclure l’automatisation de la configuration dans votre projet.

## Étape suivante

* [3 - Rubriques de mise en cache avancée](chapter-3.md)
