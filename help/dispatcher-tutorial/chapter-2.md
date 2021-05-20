---
title: '"Chapitre 2 - infrastructure de Dispatcher"'
description: Comprendre la topologie de publication et de Dispatcher. Découvrez les topologies et configurations les plus courantes.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1866'
ht-degree: 0%

---


# Chapitre 2 - Infrastructure

## Configuration d’une infrastructure de mise en cache

Nous avons présenté la topologie de base d’un système de publication et d’un Dispatcher au chapitre 1 de cette série. Un ensemble de serveurs de publication et de Dispatcher peut être configuré dans de nombreuses variantes, selon la charge attendue, la topologie de votre ou vos centres de données et les propriétés de basculement souhaitées.

Nous allons esquisser les topologies les plus courantes et décrire les avantages et les endroits où ils ne sont pas à la hauteur. La liste - bien sûr - ne peut jamais être exhaustive. La seule limite est votre imagination.

### Configuration &quot;héritée&quot;

Dans les premiers jours, le nombre de visiteurs potentiels était faible, le matériel coûteux et les serveurs web n&#39;étaient pas considérés comme aussi essentiels qu&#39;aujourd&#39;hui. Une configuration courante consistait à faire en sorte qu’un Dispatcher serve d’équilibreur de charge et de cache devant deux systèmes de publication ou plus. Le serveur Apache au coeur de Dispatcher était très stable et, dans la plupart des paramètres, capable de répondre à un bon nombre de requêtes.

![Configuration de Dispatcher &quot;héritée&quot; - Pas très courante selon les normes actuelles](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuration de Dispatcher &quot;héritée&quot; - Pas très courante selon les normes actuelles*

<br> 

C’est de là que le Dispatcher a reçu son nom : C&#39;était en gros envoyer des requêtes. Cette configuration n’est plus très courante, car elle ne peut plus répondre aux exigences de performances et de stabilité plus élevées requises aujourd’hui.

### Configuration multipage

De nos jours, une topologie légèrement différente est plus courante. Une topologie à plusieurs pattes comporterait un Dispatcher par serveur de publication. Un équilibreur de charge dédié (matériel) se trouve devant l’infrastructure AEM qui distribue les requêtes à ces deux (ou plusieurs) pattes :

![Configuration moderne du Dispatcher &quot;standard&quot; : facile à gérer et à gérer](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuration moderne du Dispatcher &quot;standard&quot; : facile à gérer et à gérer*

<br> 

Voici les raisons de ce type de configuration :

1. En moyenne, les sites web diffusent beaucoup plus de trafic que par le passé. Il est donc nécessaire d&#39;augmenter l&#39;&quot;infrastructure Apache&quot;.

2. La configuration &quot;héritée&quot; ne fournissait pas de redondance au niveau du Dispatcher. Si le serveur Apache était arrêté, l’ensemble du site web était inaccessible.

3. Les serveurs Apache sont bon marché. Ils sont basés sur l’open source et, étant donné que vous disposez d’un centre de données virtuel, ils peuvent être configurés très rapidement.

4. Cette configuration permet d’obtenir facilement un scénario de mise à jour &quot;variable&quot; ou &quot;échelonné&quot;. Il vous suffit d’arrêter Dispatcher 1 lors de l’installation d’un nouveau package logiciel sur la publication 1. Une fois l’installation terminée et que vous avez suffisamment testé la publication 1 sur le réseau interne, vous nettoyez le cache sur Dispatcher 1 et redémarrez-le tout en arrêtant Dispatcher 2 pour maintenance de la publication 2.

5. L’invalidation du cache devient très facile et déterministe dans cette configuration. Comme un seul système de publication est connecté à un Dispatcher, il n’y a qu’un seul Dispatcher à invalider. L’ordre et le timing de l’invalidation sont triviaux.

### Configuration &quot;Échelle&quot;

Les serveurs Apache sont bon marché et faciles à configurer, pourquoi ne pas augmenter un peu plus ce niveau. Pourquoi ne pas avoir deux ou plusieurs dispatchers devant chaque serveur de publication ?

![Configuration de &quot;mise à l’échelle&quot; : comporte des zones d’application, mais aussi des limites et des avertissements.](assets/chapter-2/scale-out-setup.png)

*Configuration de &quot;mise à l’échelle&quot; : comporte des zones d’application, mais aussi des limites et des avertissements.*

<br> 

Vous pouvez absolument le faire ! Et il y a beaucoup de scénarios d&#39;application valides pour cette configuration. Mais il y a aussi des limites et des complexités que vous devriez considérer.

#### Invalidation

Chaque système de publication est connecté à une multitude de Dispatchers. Chacun d’eux doit être invalidé lorsque le contenu a été modifié.

#### Maintenance

Il va sans dire que la configuration initiale des systèmes Dispatcher et Publish est un peu plus complexe. Mais gardez également à l’esprit que l’effort d’une version &quot;mobile&quot; est également un peu plus important. Les systèmes AEM peuvent et doivent être mis à jour lors de l’exécution. Mais il est sage de ne pas le faire pendant qu&#39;ils traitent activement des demandes. En règle générale, vous ne souhaitez mettre à jour qu’une partie des systèmes de publication, tandis que les autres servent toujours activement le trafic, puis, après le test, basculez vers l’autre partie. Si vous avez de la chance et que vous pouvez accéder à l’équilibreur de charge dans votre processus de déploiement, vous pouvez désactiver ici le routage vers les serveurs en cours de maintenance. Si vous utilisez un équilibreur de charge partagé sans accès direct, vous préférez arrêter les dispatchers de l’élément Publier que vous souhaitez mettre à jour. Plus il y en a, plus vous aurez à fermer. S’il y a un grand nombre de messages et que vous planifiez des mises à jour fréquentes, il est conseillé d’effectuer une automatisation. Si vous n&#39;avez pas d&#39;outils d&#39;automatisation, la mise à l&#39;échelle est de toute façon une mauvaise idée.

Dans un projet précédent, nous avons utilisé une autre astuce pour supprimer un système de publication de l’équilibrage de charge sans avoir un accès direct au répartiteur de charge lui-même.

L’équilibreur de charge &quot;pings&quot;, une page spécifique pour voir si le serveur est en cours d’exécution. Un choix trivial consiste généralement à envoyer un ping à la page d’accueil. Mais si vous voulez utiliser le ping pour signaler au répartiteur de charge de ne pas équilibrer le trafic, vous choisiriez autre chose. Vous créez un modèle ou un servlet dédié qui peut être configuré pour répondre avec `"up"` ou `"down"` (dans le corps ou sous forme de code de réponse http). La réponse de cette page ne doit bien sûr pas être mise en cache dans le Dispatcher. Elle est donc toujours récupérée de manière fraîche du système de publication. Maintenant, si vous configurez l’équilibreur de charge pour vérifier ce modèle ou servlet, vous pouvez facilement laisser l’option Publier &quot;faire semblant&quot; qu’il est hors service. Il ne fait pas partie de l’équilibrage de charge et peut être mis à jour.

#### Distribution mondiale

&quot;Distribution mondiale&quot; est une configuration de &quot;mise à l’échelle&quot; où vous avez plusieurs Dispatchers devant chaque système de publication, désormais répartis dans le monde entier pour être plus proches du client et offrir de meilleures performances. Bien sûr, dans ce scénario, vous n&#39;avez pas d&#39;équilibreur de charge central mais un schéma d&#39;équilibrage de charge DNS et géo-IP.

>[!NOTE]
>
>En fait, vous créez une sorte de réseau de distribution de contenu (CDN) avec cette approche. Vous devriez donc envisager d’acheter une solution CDN standard plutôt que de la créer vous-même. La création et la maintenance d’un réseau de diffusion de contenu personnalisé n’est pas chose facile.

#### Mise à l’échelle horizontale

Même dans un centre de données local, une topologie de &quot;mise à l’échelle&quot; avec plusieurs dispatchers devant chaque système de publication présente certains avantages. Si vous constatez des goulets d’étranglement sur les performances des serveurs Apache en raison d’un trafic élevé (et d’un bon taux d’accès au cache) et que vous ne pouvez plus augmenter le matériel (en ajoutant des processeurs, de la mémoire vive et des Disques plus rapides), vous pouvez améliorer les performances en ajoutant des Dispatchers. On parle alors de &quot;mise à l’échelle horizontale&quot;. Toutefois, cela comporte des limites, en particulier lorsque vous invalidez fréquemment le trafic. L&#39;effet est présenté dans la section suivante.

#### Limites de la topologie de mise à l’échelle

L’ajout de serveurs proxy doit normalement améliorer les performances. Cependant, il existe des scénarios où l’ajout de serveurs peut en fait réduire les performances. Comment ? Supposons que vous ayez un portail d’actualités, où vous présentez de nouveaux articles et de nouvelles pages chaque minute. Un Dispatcher invalide par &quot;invalidation automatique&quot; : Chaque fois qu’une page est publiée, toutes les pages du cache du même site sont invalidées. Il s’agit d’une fonctionnalité utile. Nous l’avons décrite dans le [chapitre 1](chapter-1.md) de cette série, mais cela signifie également que lorsque vous effectuez fréquemment des modifications sur votre site web, vous invalidez assez souvent le cache. Si vous ne disposez que d’un seul Dispatcher par instance de publication, le premier visiteur qui demande une page déclenche une nouvelle mise en cache de cette page. Le deuxième visiteur obtient déjà la version mise en cache.

Si vous disposez de deux dispatchers, le deuxième visiteur a 50 % de chances que la page ne soit pas mise en cache, et il connaîtra alors une latence plus importante lorsque cette page est rendue à nouveau. Le fait d’avoir encore plus de Dispatcher par publication rend les choses encore plus difficiles. Ce qui se passe, c’est que le serveur de publication reçoit plus de charge, car il doit rendre à nouveau la page pour chaque Dispatcher séparément.

![Diminution des performances dans un scénario de mise à l’échelle avec vidages fréquents du cache.](assets/chapter-2/decreased-performance.png)

*Diminution des performances dans un scénario de mise à l’échelle avec vidages fréquents du cache.*

<br> 

#### Réduction des problèmes de surdimensionnement

Vous pouvez envisager d’utiliser un stockage partagé central pour tous les dispatchers ou de synchroniser les systèmes de fichiers des serveurs Apache afin d’atténuer les problèmes. Nous ne pouvons fournir qu’une expérience de première main limitée, mais nous préparons à ce que cela ajoute à la complexité du système et puisse introduire une toute nouvelle classe d’erreurs.

Nous avons fait quelques expériences avec NFS, mais NFS présente d’énormes problèmes de performances en raison du verrouillage du contenu. Cela a en fait diminué la performance globale.

**Conclusion**  - Le partage d’un système de fichiers commun entre plusieurs dispatchers n’est PAS une approche recommandée.

Si vous rencontrez des problèmes de performances, redimensionnez les instances de publication et de Dispatcher de manière égale afin d’éviter un pic de charge sur les instances de l’éditeur. Il n’existe pas de règle d’or concernant le ratio Publier/Dispatcher ; il dépend fortement de la distribution des requêtes et de la fréquence des publications et des invalidations du cache.

Si la latence ressentie par un visiteur vous préoccupe également, envisagez d’utiliser un réseau de diffusion de contenu, de récupérer le cache, de préparer le cache, de définir un délai de grâce comme décrit dans le [chapitre 1](chapter-1.md) de cette série ou de vous référer à quelques idées avancées de [Partie 3](chapter-3.md).

### Configuration &quot;Cross Connected&quot;

Une autre configuration que nous avons vue de temps en temps est la configuration &quot;inter-connectée&quot; : Les instances de publication n’ont pas de Dispatcher dédié, mais tous les Dispatcher sont connectés à tous les systèmes de publication.

![Topologie inter-connectée : Plus de redondance et plus de complexité](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topologie inter-connectée : Une redondance et une complexité accrues.*

À première vue, cela donne une plus grande redondance pour un budget relativement modeste. Lorsque l’un des serveurs Apache est hors service, deux systèmes de publication peuvent toujours effectuer le rendu. En outre, si l’un des systèmes de publication se bloque, deux Dispatcher servent toujours la charge mise en cache.

Mais cela a un prix.

Tout d&#39;abord, enlever une jambe pour la maintenance est assez lourd. En fait, c&#39;est à cela que ce plan a été conçu : d&#39;être plus résilient et de rester opérationnel par tous les moyens possibles. Nous avons vu des plans de maintenance complexes sur la façon de gérer cela. Reconfigurez Dispatcher 2 en premier, supprimant la connexion croisée. Redémarrage de Dispatcher 2. Arrêt de Dispatcher 1, mise à jour de la publication 1, ... etc. Vous devriez réfléchir attentivement si cela peut monter à plus de deux pattes. Vous en arriverez à la conclusion, qu&#39;en fait cela augmente la complexité, les coûts et est une source formidable d&#39;erreur humaine. Il serait préférable d&#39;automatiser cela. Donc mieux vérifier, si vous avez réellement les ressources humaines pour inclure cette tâche d&#39;automatisation dans le planning de votre projet. Bien que vous puissiez économiser certains coûts matériels avec cela, vous pourriez dépenser deux fois plus en personnel informatique.

Deuxièmement, il se peut qu’une application utilisateur s’exécute sur l’AEM qui requiert une connexion. Vous utilisez des sessions persistantes pour vous assurer qu’un utilisateur est toujours servi à partir de la même instance AEM afin que vous puissiez conserver l’état de session sur cette instance. Avec cette configuration interconnectée, vous devez vous assurer que les sessions persistantes fonctionnent correctement sur l’équilibreur de charge et sur les dispatchers. Pas impossible, mais vous devez en être conscient et ajouter des heures de configuration et de test supplémentaires, qui, encore une fois, peuvent vous faire économiser du matériel.

### Conclusion

Nous vous déconseillons d’utiliser ce schéma de connexion croisée comme option par défaut. Cependant, si vous décidez de l’utiliser, vous devrez soigneusement évaluer les risques et les coûts cachés et planifier d’inclure l’automatisation de la configuration dans votre projet.

## Étape suivante

* [3 - Rubriques de mise en cache avancée](chapter-3.md)
