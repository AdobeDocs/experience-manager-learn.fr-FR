---
title: AEM vidage du dispatcher
description: Découvrez comment AEM invalide les anciens fichiers cache du Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 0%

---


# URL Vanity de Dispatcher

[Table des matières](./overview.md)

[&lt;- Précédent : Utilisation et compréhension des variables](./variables.md)

Ce document fournit des conseils sur la façon dont la purge se produit et explique le mécanisme qui exécute la purge du cache et l’invalidation.


## Fonctionnement

### Ordre des opérations

Le workflow type est mieux décrit lorsque les auteurs de contenu activent une page, lorsque l’éditeur reçoit le nouveau contenu, il déclenche une demande de purge vers Dispatcher, comme illustré dans le diagramme suivant :
![L’auteur active le contenu, ce qui déclenche l’envoi par l’éditeur d’une demande de vidage vers Dispatcher.](assets/disp-flushing/dispatcher-flushing-order-of-events.png "dispatcher-flushing-order-of-events")
Cet enchaînement d’événements souligne que nous ne vidons les éléments que lorsqu’ils sont nouveaux ou ont changé.  Cela permet de s’assurer que le contenu a été reçu par l’éditeur avant d’effacer le cache afin d’éviter les conditions de concurrence dans lesquelles le vidage peut se produire avant que les modifications ne puissent être récupérées auprès de l’éditeur.

## Agents de réplication

Sur l’instance de création, un agent de réplication est configuré pour indiquer à l’éditeur que lorsqu’un élément est activé, il déclenche l’envoi du fichier et de toutes ses dépendances à l’éditeur.

Lorsque l’éditeur reçoit le fichier, un agent de réplication est configuré pour pointer vers Dispatcher qui se déclenche sur l’événement à la réception.  Il sérialisera ensuite une requête de purge et la publiera dans Dispatcher.

### AGENT DE RÉPLICATION DE L’AUTEUR

Voici quelques exemples de captures d’écran d’un agent de réplication standard configuré
![capture d’écran de l’agent de réplication standard à partir de la page web d’AEM /etc/replication.html](assets/disp-flushing/author-rep-agent-example.png "author-rep-agent-example")

Il existe généralement 1 ou 2 agents de réplication configurés sur l’auteur pour chaque éditeur vers lequel ils répliquent du contenu.

Tout d’abord, il s’agit de l’agent de réplication standard qui envoie les activations de contenu à .

Le second est l&#39;agent inverse.  Cette option est facultative et configurée pour vérifier dans chaque boîte d’envoi des éditeurs s’il existe un nouveau contenu à extraire dans l’auteur en tant qu’activité de réplication inverse.

### AGENT DE RÉPLICATION DE L’ÉDITEUR

Voici un exemple de captures d’écran d’un agent de réplication de purge standard configuré
![capture d’écran de l’agent de réplication de vidage standard à partir de la page web AEM /etc/replication.html](assets/disp-flushing/publish-flush-rep-agent-example.png "publish-flush-rep-agent-example")

### RÉPLICATION DE FLUX DE DISPATCHER REÇANT UN HÔTE VIRTUEL

Le module de Dispatcher recherche des en-têtes particuliers à connaître lorsqu’une demande de POST doit être transmise aux rendus AEM ou s’il s’agit d’une demande sérialisée en tant que demande de purge et qui doit être traitée par le gestionnaire de Dispatcher lui-même.

Voici une capture d’écran de la page de configuration qui affiche ces valeurs :
![image de l’onglet des paramètres de l’écran de configuration principal avec le type de sérialisation affiché comme vidage de Dispatcher](assets/disp-flushing/disp-flush-agent1.png "disp-flush-agent1")

La page des paramètres par défaut affiche la variable `Serialization Type` as `Dispatcher Flush` et définit le niveau d’erreur

![Capture d’écran de l’onglet Transport de l’agent de réplication.  Cela affiche l’URI vers lequel envoyer la requête de purge.  /dispatcher/invalidate.cache](assets/disp-flushing/disp-flush-agent2.png "disp-flush-agent2")

Sur le `Transport` vous pouvez voir la variable `URI` définie pour pointer l’adresse IP du Dispatcher qui recevra les requêtes de purge.  Chemin d’accès `/dispatcher/invalidate.cache` n’est pas la manière dont le module détermine s’il s’agit d’une purge. Il s’agit uniquement d’un point de terminaison évident que vous pouvez voir dans le journal d’accès pour savoir qu’il s’agissait d’une requête de purge.  Sur le `Extended` Nous allons passer en revue les éléments qui sont présents pour vérifier qu’il s’agit d’une demande de vidage envoyée au module de Dispatcher.

![Capture d’écran de l’onglet Étendu de l’agent de réplication.  Notez les en-têtes qui sont envoyés avec la demande de POST envoyée pour indiquer à Dispatcher de vider](assets/disp-flushing/disp-flush-agent3.png "disp-flush-agent3")

Le `HTTP Method` pour les requêtes de purge, il s’agit simplement d’une `GET` requête avec des en-têtes de requête spéciaux :
- CQ-Action
   - Cette opération utilise une variable AEM basée sur la requête et la valeur est généralement *activer ou supprimer*
- CQ-Handle
   - Cette opération utilise une variable d’AEM basée sur la requête et la valeur est généralement le chemin d’accès complet à l’élément vidé, par exemple. `/content/dam/logo.jpg`
- CQ-Path
   - Cette opération utilise une variable d’AEM basée sur la requête et la valeur est généralement le chemin d’accès complet à l’élément en cours de purge, par exemple. `/content/dam`
- Hôte
   - C’est là que le `Host` L’en-tête est usurpé pour cibler une `VirtualHost` qui est configuré sur le serveur web Apache du dispatcher (`/etc/httpd/conf.d/enabled_vhosts/aem_flush.vhost`).  Il s’agit d’une valeur codée en dur qui correspond à une entrée dans la variable `aem_flush.vhost` fichier `ServerName` ou `ServerAlias`

![Écran d’un agent de réplication standard indiquant que l’agent de réplication réagit et se déclenche lorsque de nouveaux éléments ont été reçus d’un événement de réplication de la part de l’auteur publiant le contenu](assets/disp-flushing/disp-flush-agent4.png "disp-flush-agent4")

Sur le `Triggers` nous prendrons note des déclencheurs activés que nous utilisons et de ce qu’ils sont.

- `Ignore default`
   - Cette option est activée de sorte que l’agent de réplication ne soit pas déclenché lors de l’activation d’une page.  Lorsqu’une instance d’auteur modifie une page, cela déclenche une purge.  Parce qu&#39;il s&#39;agit d&#39;un éditeur, nous ne voulons pas déclencher ce type d&#39;événement.
- `On Receive`
   - Lorsqu’un nouveau fichier est reçu, nous voulons déclencher une purge.  Ainsi, lorsque l’auteur nous envoie un fichier mis à jour, nous déclenchons et envoyons une demande de purge à Dispatcher.
- `No Versioning`
   - Nous vérifions cela pour éviter que l’éditeur ne génère de nouvelles versions, car un nouveau fichier a été reçu.  Nous allons simplement remplacer le fichier que nous avons et nous fier à l&#39;auteur pour suivre les versions au lieu de l&#39;éditeur.

Maintenant, si nous regardons à quoi ressemble une requête de purge standard sous la forme d’une `curl` command

```
$ curl \ 
-H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/dam/logo.jpg" \ 
-H "CQ-Path: /content/dam/" \ 
-H "Content-Length: 0" \  
-H "Content-Type: application/octect-stream" \ 
-H "Host: flush" \ 
http://10.43.0.32:80/dispatcher/invalidate.cache
```

Cet exemple de vidage vide `/content/dam` chemin en mettant à jour la variable `.stat` dans ce répertoire.

## Le `.stat` fichier

Le mécanisme de purge est simple par nature et nous voulons expliquer l&#39;importance de `.stat` fichiers générés à la racine du document dans laquelle les fichiers de cache sont créés.

Dans le `.vhost` et `_farm.any` Nous configurons une directive racine de document pour spécifier l’emplacement du cache et l’emplacement à partir duquel stocker/servir les fichiers lorsqu’une demande d’un utilisateur final arrive.

Si vous deviez exécuter la commande suivante sur votre serveur Dispatcher, vous commenceriez à trouver `.stat` files

```
$ find /mnt/var/www/html/ -type f -name ".stat"
```

Voici un diagramme de l’apparence de cette structure de fichiers lorsque vous avez des éléments dans le cache et qu’une demande de purge a été envoyée et traitée par le module de Dispatcher.

![fichiers de statut mélangés avec du contenu et des dates affichés avec les niveaux de statistiques affichés](assets/disp-flushing/dispatcher-statfiles.png "dispatcher-statfiles")

### NIVEAU DU FICHIER STADE

Notez que dans chaque répertoire, une `.stat` fichier présent.  Il s’agit d’un indicateur indiquant qu’une purge a eu lieu.  Dans l’exemple ci-dessus, `statfilelevel` a été défini sur `3` dans le fichier de configuration de ferme correspondant.

Le `statfilelevel` indique le nombre de dossiers que le module parcourra et mettra à jour un `.stat` fichier .  Le fichier .stat est vide. Il ne s’agit rien de plus qu’un nom de fichier avec un horodatage et peut même être créé manuellement, mais il exécute la commande tactile sur la ligne de commande du serveur Dispatcher.

Si le paramètre de niveau de fichier stat est défini sur une valeur trop élevée, chaque requête de purge traverse l’arborescence de répertoires pour toucher les fichiers .stat.  Cela peut devenir un accès aux performances majeur sur les grandes arborescences du cache et avoir une incidence sur les performances globales de votre Dispatcher.

Si vous définissez ce niveau de fichier sur une valeur trop basse, une requête de purge peut effacer plus que prévu.  Ce qui, à son tour, provoquerait l’exécution du cache plus souvent avec moins de requêtes diffusées à partir du cache et peut entraîner des problèmes de performances.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Définissez la variable `statfilelevel` à un niveau raisonnable.  Examinez la structure de vos dossiers et assurez-vous qu’elle est configurée pour permettre des vidages concis sans avoir à parcourir trop de répertoires.   Testez-le et assurez-vous qu’il correspond à vos besoins lors d’un test de performance du système.

Un bon exemple est un site qui prend en charge les langues.  L’arborescence de contenu classique contiendra les répertoires suivants :

`/content/brand1/en/us/`

Dans cet exemple, utilisez un paramètre de niveau fichier stat de 4.  Cela vous assure lorsque vous videz du contenu qui se trouve sous la balise <b>`us`</b> pour empêcher le vidage des dossiers de langue.
</div>

### GESTION DE L’HORODATAGE DU FICHIER STADE

Lorsqu’une demande de contenu entre dans la même routine se produit

1. Horodatage de la variable `.stat` est comparé à l’horodatage du fichier demandé.
2. Si la variable `.stat` est plus récent que le fichier demandé. Il supprime le contenu mis en cache et en récupère un nouveau dans AEM et le met en cache.  Ensuite, diffuse le contenu
3. Si la variable `.stat` est plus ancien que le fichier demandé, il sait alors que le fichier est neuf et peut servir le contenu.

### GESTION DU CACHE - EXEMPLE 1

Dans l’exemple ci-dessus, une requête pour le contenu `/content/index.html`

L’heure de la variable `index.html` Le fichier est 2019-11-01 à 18h21

L’heure du `.stat` Le fichier est 2019-11-01 @ 22h22

En comprenant ce que nous avons lu ci-dessus, vous pouvez constater que le fichier d’index est plus récent que le `.stat` et le fichier serait transmis du cache à l’utilisateur final qui l’a demandé.

### GESTION DU CACHE - EXEMPLE 2

Dans l’exemple ci-dessus, une requête pour le contenu `/content/dam/logo.jpg`

L’heure de la variable `logo.jpg` Le fichier est 2019-10-31 @ 13h13

L’heure du `.stat` Le fichier est 2019-11-01 @ 22h22

Comme vous pouvez le constater dans cet exemple, le fichier est plus ancien que le `.stat` et seront supprimés et un nouveau extrait de l’AEM pour le remplacer dans le cache avant d’être transmis à l’utilisateur final qui l’a demandé.

## Paramètres du fichier de ferme

La documentation est disponible ici pour l’ensemble des options de configuration : [https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#configuring-dispatcher_configuring-the-dispatcher-cache-cache)

Nous allons mettre en évidence quelques-uns d’entre eux qui se rapportent à la purge du cache.

### Fermes de vidage

Il existe deux clés : `document root` répertoires qui mettront en cache les fichiers à partir du trafic de l’auteur et de l’éditeur.  Pour que ces répertoires soient à jour avec le nouveau contenu, nous devons vider le cache.  Ces demandes de vidage ne souhaitent pas être mêlées à vos configurations normales de fermes de trafic client qui peuvent rejeter la demande ou faire quelque chose d’indésirable.  Au lieu de cela, nous fournissons deux fermes de vidage pour cette tâche :

- `/etc/httpd.conf.d/available_farms/001_ams_author_flush_farm.any`
- `/etc/httpd.conf.d/available_farms/001_ams_publish_flush_farm.any`

Ces fichiers de fermes ne font rien d’autre que vider les répertoires racine du document.

```
/publishflushfarm {  
	/virtualhosts {
		"flush"
	}
	/cache {
		/docroot "${PUBLISH_DOCROOT}"
		/statfileslevel "${DEFAULT_STAT_LEVEL}"
		/rules {
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any"
		}
		/invalidate {
			/0000 {
				/glob "*"
				/type "allow"
			}
		}
		/allowedClients {
			/0000 {
				/glob "*.*.*.*"
				/type "deny"
			}
			$include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any"
		}
	}
}
```

### Racine du document

Cette entrée de configuration se trouve dans la section suivante du fichier de ferme :

```
/myfarm { 
    /cache { 
        /docroot
```

Vous spécifiez le répertoire dans lequel vous souhaitez que Dispatcher soit renseigné et géré en tant que répertoire de cache.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
Ce répertoire doit correspondre au paramètre racine du document Apache pour le domaine que votre serveur web est configuré pour utiliser.

Avoir des dossiers docroot imbriqués pour chaque ferme qui vivent des sous-dossiers de la racine du document Apache est une mauvaise idée pour de nombreuses raisons.
</div>

### Niveau de fichier d’état

Cette entrée de configuration se trouve dans la section suivante du fichier de ferme :

```
/myfarm { 
    /cache { 
        /statfileslevel
```

Ce paramètre évalue la profondeur `.stat` Les fichiers doivent être générés lorsqu’une requête de purge arrive.

`/statfileslevel` définissez sur le nombre suivant avec la racine du document de `/var/www/html/` aurait les résultats suivants lors de la purge `/content/dam/brand1/en/us/logo.jpg`

- 0 - Les fichiers .stat suivants seraient créés :
   - `/var/www/html/.stat`
- 1 - Les fichiers .stat suivants seraient créés :
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
- 2 - Les fichiers .stat suivants seraient créés :
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
- 3 - Les fichiers .stat suivants seraient créés :
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
- 4 - Les fichiers .stat suivants seraient créés :
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/dam/brand1/en/.stat`
- 5 - Les fichiers .stat suivants seraient créés :
   - `/var/www/html/.stat`
   - `/var/www/html/content/.stat`
   - `/var/www/html/content/dam/.stat`
   - `/var/www/html/content/dam/brand1/.stat`
   - `/var/www/html/content/damn/brand1/en/.stat`
   - `/var/www/html/content/damn/brand1/en/us/.stat`


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>

Gardez à l’esprit que lorsque la poignée de main d’horodatage se produit, elle recherche la plus proche `.stat` fichier .

avoir une `.stat` niveau de fichier 0 et un fichier stat uniquement à l’adresse `/var/www/html/.stat` signifie que le contenu qui vit sous `/var/www/html/content/dam/brand1/en/us/` Recherchez le plus proche `.stat` et parcourir 5 dossiers pour trouver le seul `.stat` qui existe au niveau 0 et qui compare des dates à cela.  Cela signifie qu’une purge à ce niveau élevé invaliderait essentiellement tous les éléments mis en cache.
</div>

### Invalidation autorisée

Cette entrée de configuration se trouve dans la section suivante du fichier de ferme :

```
/myfarm { 
    /cache { 
        /allowedClients {
```

Dans cette configuration, vous placez une liste d’adresses IP autorisées à envoyer des requêtes de purge.  Si une requête de purge arrive dans Dispatcher, elle doit provenir d’une adresse IP de confiance.  Si vous avez mal configuré ou si vous envoyez une requête de purge à partir d’une adresse IP non approuvée, l’erreur suivante s’affichera dans le fichier journal :

```
[Mon Nov 11 22:43:05 2019] [W] [pid 3079 (tid 139859875088128)] Flushing rejected from 10.43.0.57
```

### Règles d’invalidation

Cette entrée de configuration se trouve dans la section suivante du fichier de ferme :

```
/myfarm { 
    /cache { 
        /invalidate {
```

Ces règles indiquent généralement quels fichiers peuvent être invalidés avec une requête de purge.

Pour éviter que des fichiers importants ne soient invalidés avec l’activation d’une page, vous pouvez appliquer des règles qui spécifient quels fichiers peuvent être invalidés et lesquels doivent être invalidés manuellement.  Voici un exemple de configuration qui autorise uniquement l’invalidation des fichiers HTML :

```
/invalidate { 
   /0000 { /glob "*" /type "deny" } 
   /0001 { /glob "*.html" /type "allow" } 
}
```

## Tests/dépannage

Lorsque vous activez une page et que vous obtenez le feu vert indiquant que l’activation de la page a réussi, vous devez vous attendre à ce que le contenu que vous avez activé soit également vidé du cache.

Vous actualisez votre page et affichez les anciennes fonctionnalités ! what!? il y avait un feu vert?!

Suivons quelques étapes manuelles du processus de purge pour nous donner une idée de ce qui pourrait être mauvais.  Dans le shell de l’éditeur, exécutez la requête de purge suivante à l’aide de curl :

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "CQ-Path: /content/<PATH TO ITEM TO FLUSH>" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://<DISPATCHER IP ADDRESS>/dispatcher/invalidate.cache
```

Exemple de requête de purge de test

```
$ curl -H "CQ-Action: Activate" \ 
-H "CQ-Handle: /content/customer/en-us" \ 
-H "CQ-Path: /content/customer/en-us" \ 
-H "Content-Length: 0" -H "Content-Type: application/octet-stream" \ 
-H "Host: flush" \ 
http://169.254.196.222/dispatcher/invalidate.cache
```

Une fois que vous avez déclenché la commande de requête vers Dispatcher, vous souhaiterez voir ce qui est fait dans les journaux et ce qui est fait avec le `.stat files`.  Suivez le fichier journal et vous devriez voir les entrées suivantes pour confirmer que la demande de purge a atteint le module de Dispatcher.

```
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Activation detected: action=Activate [/content/dam/logo.jpg] 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] Touched /mnt/var/www/html/content/dam/.stat 
[Wed Nov 13 16:54:12 2019] [I] [pid 19173:tid 140542721578752] "GET /dispatcher/invalidate.cache" 200 purge [publishfarm/-] 0ms
```

Maintenant que le module a été récupéré et que la demande de vidage a été acquittée, nous devons voir comment il a affecté la variable `.stat` fichiers .  Exécutez la commande suivante et observez la mise à jour des horodatages lorsque vous lancez une autre purge :

```
$ watch -n 3 "find /mnt/var/www/html/ -type f -name ".stat" | xargs ls -la $1"
```

Comme vous pouvez le voir à partir de la sortie de la commande, les horodatages de la `.stat` files

```
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 16:54 /mnt/var/www/html/.stat
```

Maintenant, si nous réexécutons la purge, vous verrez la mise à jour des horodatages.

```
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/dam/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/content/.stat 
-rw-r--r--. 1 apache apache 0 Nov 13 17:17 /mnt/var/www/html/.stat
```

Comparons nos horodatages de contenu à notre `.stat` horodatages des fichiers

```
$ stat /mnt/var/www/html/content/customer/en-us/.stat 
  File: `.stat' 
  Size: 0           Blocks: 0          IO Block: 4096   regular empty file 
Device: ca90h/51856d    Inode: 17154125    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-13 16:22:31.000000000 -0400 
Modify: 2019-11-13 16:22:31.000000000 -0400 
Change: 2019-11-13 16:22:31.000000000 -0400 
 
$ stat /mnt/var/www/html/content/customer/en-us/logo.jpg 
File: `logo.jpg' 
  Size: 15856           Blocks: 32          IO Block: 4096   regular file 
Device: ca90h/51856d    Inode: 9175290    Links: 1 
Access: (0644/-rw-r--r--)  Uid: (   48/  apache)   Gid: (   48/  apache) 
Access: 2019-11-11 22:41:59.642450601 +0000 
Modify: 2019-11-11 22:41:59.642450601 +0000 
Change: 2019-11-11 22:41:59.642450601 +0000
```

Si vous observez l’un des horodatages, vous remarquerez que le contenu a une heure plus récente que la variable `.stat` fichier qui indique au module de servir le fichier à partir du cache, car il est plus récent que le fichier `.stat` fichier .

En clair, quelque chose a mis à jour les horodatages de ce fichier qui ne le qualifient pas pour être &quot;purgé&quot; ou remplacé.

[Suivant -> URL Vanity](./disp-vanity-url.md)