---
title: Explication des fichiers de configuration de Dispatcher
description: Comprendre les fichiers de configuration, les conventions d’affectation des noms, etc.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# Explication des fichiers de configuration

[Table des matières](./overview.md)

[&lt;- Précédent : Mise en page de base du fichier](./basic-file-layout.md)

Ce document ventile et explique chacun des fichiers de configuration déployés dans un serveur Dispatcher standard intégré configuré dans Adobe Managed Services. Leur utilisation, convention de nommage, etc...

## Convention d’affectation de nom

Le serveur web Apache ne se soucie pas vraiment de l’extension de fichier d’un fichier lorsqu’il est ciblé avec une `Include` ou `IncludeOptional` .  Nommer correctement les noms avec des noms qui éliminent les conflits et la confusion permet d’obtenir une <b>ton</b>. Les noms utilisés décrivent la portée de l’application du fichier, ce qui facilite la vie. Si tout est nommé `.conf` ça devient vraiment déroutant. Nous voulons éviter les fichiers et les extensions mal nommés.  Vous trouverez ci-dessous une liste des différentes extensions de fichier personnalisées et des conventions d’appellation utilisées dans un Dispatcher configuré par AMS classique.

## Fichiers contenus dans conf.d/

| File | Destination du fichier | Description |
| ---- | ---------------- | ----------- |
| FILENAME`.conf` | `/etc/httpd/conf.d/` | Une installation Enterprise Linux par défaut utilise cette extension de fichier et inclut le dossier comme emplacement pour remplacer les paramètres déclarés dans httpd.conf et vous permettre d’ajouter des fonctionnalités supplémentaires à un niveau global dans Apache. |
| FILENAME`.vhost` | Intermédiaire : `/etc/httpd/conf.d/available_vhosts/`<br>Principal : `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b> Les fichiers .vhost ne doivent pas être copiés dans le dossier enabled_vhosts, mais utiliser des liens symboliques vers un chemin relatif au fichier available_vhosts/\*.vhost .</div></u><br><br> | Les fichiers \*.vhost (Virtual Host) sont `<VirtualHosts>`  des entrées correspondant aux noms d’hôtes et permettant à Apache de gérer chaque trafic de domaine avec des règles différentes. Dans la `.vhost` fichier, d’autres fichiers tels que `rewrites`, `whitelisting`, `etc` sera inclus. |
| FILENAME`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` magasin de fichiers `mod_rewrite` règles à inclure et à utiliser explicitement par une `vhost` fichier |
| FILENAME`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` Les fichiers sont inclus dans la variable `*.vhost` fichiers . Elle contient une expression régulière IP ou autorise les règles de refus à autoriser l’liste autorisée d’adresses IP. Si vous essayez de restreindre l’affichage d’un hôte virtuel en fonction des adresses IP, vous allez générer l’un de ces fichiers et l’inclure à partir de votre `*.vhost` fichier |

## Fichiers contenus dans conf.modules.d/

| Fichier | Destination du fichier | Description |
| --- | --- | --- |
| FILENAME`.any` | `/etc/httpd/conf.dispatcher.d/` | Le module Apache de Dispatcher AEM sources ses paramètres à partir de `*.any` fichiers . Le fichier d’inclusion parent par défaut est : `conf.dispatcher.d/dispatcher.any` |
| FILENAME`_farm.any` | Intermédiaire : `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Principal : `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b> ces fichiers de fermes ne doivent pas être copiés dans la variable `enabled_farms` mais utilisez `symlinks` à un chemin relatif vers la propriété `available_farms/*_farm.any` fichier </div> <br/>`*_farm.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/dispatcher.any` fichier . Ces fichiers de fermes parents existent pour contrôler le comportement du module pour chaque type de rendu ou de site web. Les fichiers sont créés dans la variable `available_farms` et activé avec un `symlink` dans la `enabled_farms` répertoire .  <br/>Elle les inclut automatiquement par nom à partir du `dispatcher.any` fichier .<br/><b>Ligne de base</b> Les fichiers de fermes commencent par `000_` pour vous assurer qu’elles sont chargées en premier.<br><b>Personnalisé</b> Les fichiers de fermes doivent être chargés après en démarrant leur modèle de nombre à l’adresse `100_` pour assurer le comportement d’inclusion approprié. |
| FILENAME`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/enabled_farms/*_farm.any` fichiers . Chaque ferme de serveurs comporte un ensemble de règles qui modifient le trafic qui doit être filtré et ne pas atteindre les moteurs de rendu. |
| FILENAME`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/enabled_farms/*_farm.any` fichiers . Ces fichiers sont une liste de noms d’hôtes ou de chemins d’accès uri à mettre en correspondance d’objets blob pour déterminer le moteur de rendu à utiliser pour diffuser cette requête. |
| FILENAME`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/enabled_farms/*_farm.any` fichiers . Ces fichiers spécifient les éléments qui sont mis en cache et ceux qui ne le sont pas. |
| FILENAME`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/enabled_farms/*_farm.any` fichiers . Ils spécifient les adresses IP autorisées à envoyer des demandes de purge et d’invalidation. |
| FILENAME`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/enabled_farms/*_farm.any` fichiers . Ils spécifient les en-têtes client à transmettre à chaque moteur de rendu. |
| FILENAME`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` Les fichiers sont inclus dans la variable `conf.dispatcher.d/enabled_farms/*_farm.any` fichiers . Ils spécifient les paramètres d’adresse IP, de port et de délai d’expiration pour chaque moteur de rendu. Un moteur de rendu correct peut être un serveur LiveCycle ou tout système AEM sur lequel Dispatcher peut récupérer/remplacer les requêtes de |

## Problèmes évités

Lorsque vous suivez la convention de dénomination, vous pouvez éviter des erreurs assez faciles à commettre qui peuvent avoir des résultats catastrophiques.  Nous allons en présenter quelques exemples.

### Exemple de problème

Comme exemple de site pour ExempleCo, deux fichiers de configuration ont été créés par les développeurs des configurations du Dispatcher.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Si la variable `vhost` est accidentellement placé dans la variable `rewrites` et le dossier `rewrites file` est placé dans la variable `vhosts` dossier.  Il semble qu’il soit déployé correctement par nom de fichier, mais Apache lance une *ERROR* et le problème ne sera pas évident immédiatement.

<b>Comment cela devient généralement un problème</b>

Si la variable `two files` sont téléchargés dans le `same` emplacement où ils peuvent `overwrite themselves` ou rendre le processus de déploiement indissociable, ce qui en fait un cauchemar.

<b>Les extensions de fichier sont identiques et incluent automatiquement</b>

Les extensions de fichier sont les mêmes et utilisent l’extension auto-incluse qu’Apache va `auto include` any `.conf` dans la plupart de ses dossiers par défaut.

<b>Comment cela devient généralement un problème</b>

Si le fichier vhost avec l’extension de `.conf` est placé dans la variable `/etc/httpd/conf.d/` le dossier tente de le charger en mémoire sur Apache, ce qui est généralement correct, mais si le fichier de règles de réécriture avec l’extension de `.conf` est placé dans la variable `/etc/httpd/conf.d/` , il sera inclus automatiquement et appliqué globalement, ce qui provoquera des résultats déroutants et indésirables.

## Résolution

Nommez les fichiers en fonction de ce qu’ils font et en toute sécurité en dehors de l’espace de noms des règles d’inclusion automatique.

S’il s’agit d’un nom de fichier d’hôte virtuel avec `.vhost` comme extension.

S’il s’agit d’un fichier de règle de réécriture, nommez-le par site.`_rewrite.rules` comme suffixe et extension. Cette convention d’affectation des noms indique clairement à quel site il sert et qu’il s’agit d’un ensemble de règles de réécriture.

S’il s’agit d’un fichier de règle de liste autorisée IP, nommez-le par description.`_whitelist.rules` comme suffixe et extension. Cette convention d’affectation des noms donnera une description de son utilité et du fait qu’il s’agit d’un ensemble de règles de correspondance d’adresses IP.

L’utilisation de ces conventions d’affectation de noms permet d’éviter des problèmes si un fichier est déplacé dans un répertoire d’inclusion automatique auquel il n’appartient pas.

Par exemple, pour placer un fichier nommé avec `.rules`, `.any`ou `.vhost` dans le dossier d’inclusion automatique de `/etc/httpd/conf.d/` n&#39;aurait aucun effet.

Si une requête de modification de déploiement indique &quot;Veuillez déployer exampleco_rewrite.rules vers les Dispatchers de production&quot;, la personne qui déploie les modifications peut déjà savoir qu’elle n’ajoute pas de nouveau site, elle se contente de mettre à jour les règles de réécriture comme indiqué par le nom du fichier.

### Inclure la commande

Lors de l’extension des fonctionnalités et des configurations dans le serveur web Apache installé sur Enterprise Linux, vous disposez de commandes d’inclusion importantes que vous souhaitez comprendre.

### Inclusions de la ligne de base Apache

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Comme le montre le diagramme ci-dessus, le binaire httpd ne regarde que le fichier httpd.conf comme fichier de configuration.  Ce fichier contient les instructions suivantes :

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Inclusions de niveau supérieur AMS

Lorsque nous avons appliqué notre norme, nous avons ajouté d’autres types de fichiers et inclus les nôtres.

Voici les répertoires de base AMS et les inclusions de niveau supérieur.
![La ligne de base AMS comprend un fichier dispatcher_vhost.conf qui inclura n’importe quel fichier avec le *.vhost du répertoire /etc/httpd/conf.d/enabled_vhosts/ .  Les éléments du répertoire /etc/httpd/conf.d/enabled_vhosts/ sont des liens symboliques vers le fichier de configuration réel qui se trouve dans /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

En partant de la ligne de base d’Apache, nous montrons comment AMS a créé des dossiers supplémentaires et des inclusions de niveau supérieur pour `conf.d` ainsi que les répertoires spécifiques aux modules imbriqués sous `/etc/httpd/conf.dispatcher.d/`

Au chargement d’Apache, l’élément `/etc/httpd/conf.modules.d/02-dispatcher.conf` et ce fichier inclut le fichier binaire. `/etc/httpd/modules/mod_dispatcher.so` dans son état d&#39;exécution.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Pour utiliser le module dans notre `<VirtualHost />` nous déposerons un fichier de configuration dans `/etc/httpd/conf.d/` named `dispatcher_vhost.conf` et dans ce fichier, vous verrez utiliser configuration des paramètres de base nécessaires au fonctionnement du module :

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Comme vous pouvez le voir ci-dessus, cela inclut le niveau supérieur `dispatcher.any` pour que notre module de Dispatcher récupère ses fichiers de configuration à partir de `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Attention au contenu de ce fichier :

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

Niveau supérieur `dispatcher.any` comprend tous les fichiers de fermes activés qui se trouvent dans `/etc/httpd/conf.dispatcher.d/enabled_farms/` avec le nom de fichier de `FILENAME_farm.any` qui suit notre convention de dénomination standard.

Plus tard dans la `dispatcher_vhost.conf` fichier mentionné précédemment, nous faisons également une instruction d’inclusion pour activer chaque fichier d’hôte virtuel activé qui se trouve dans `/etc/httpd/conf.d/enabled_vhosts/` avec le nom de fichier de `FILENAME.vhost` qui suit notre convention de dénomination standard.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

Dans chacun de nos fichiers .vhost, vous remarquerez que le module de Dispatcher est initialisé en tant que gestionnaire de fichiers par défaut pour un répertoire.  Voici un exemple de fichier .vhost pour afficher la syntaxe :

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

Une fois que les inclusions de niveau supérieur sont résolues, elles comportent d’autres sous-inclusions qui méritent d’être mentionnées.  Voici un diagramme de haut niveau sur la manière dont les fichiers de fermes de serveurs et vhosts incluent d’autres sous-éléments.

### Inclusions d’hôte virtuel AMS

![Cette image montre comment un fichier .vhost inclut des fichiers provenant de variables, de listes autorisées et de dossiers de réécriture.](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Lorsque `.vhost` fichiers provenant de `/etc/httpd/conf.d/availabled_vhosts/` créer un lien symbolique dans le répertoire `/etc/httpd/conf.d/enabled_vhosts/` Ils seront utilisés dans la configuration en cours d’exécution.

Le `.vhost` Les fichiers ont des sous-inclusions basées sur des éléments communs que nous avons trouvés.  Il s’agit de variables, de listes autorisées et de règles de réécriture.

Le `.vhost` comporte des instructions d’inclusion pour chaque fichier en fonction de l’endroit où ils doivent être inclus dans la variable `.vhost` fichier .  Voici un exemple de syntaxe d’une `.vhost` comme référence appropriée :

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

Comme vous pouvez le voir dans l’exemple ci-dessus, il existe une inclusion pour les variables nécessaires dans ce fichier de configuration qui sont utilisées ultérieurement.

Dans le fichier `/etc/httpd/conf.d/variables/weretail.vars` nous pouvons voir les variables qui sont définies :

```
Define MAIN_DOMAIN dev.weretail.com
```

Vous pouvez également voir une ligne qui comprend une liste de `_whitelist.rules` qui restreignent qui peut afficher ce contenu en fonction de différents critères de liste autorisée.  Examinons le contenu d’un des fichiers de liste autorisée. `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

Vous pouvez également voir une ligne qui comprend un ensemble de règles de réécriture.  Regardons le contenu de la `weretail_rewrite.rules` fichier :

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### Inclusions de la ferme AMS

![<FILENAME>_farms.any inclura des fichiers .any secondaires pour terminer la configuration de la ferme.  Dans cette image, vous pouvez voir qu’une ferme de serveurs va inclure chaque cache de fichiers de section de niveau supérieur, les en-têtes clients, les filtres, les rendus et les fichiers vhosts .any.](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Lorsque des fichiers FILENAME_farm.any sont `/etc/httpd/conf.dispatcher.d/available_farms/` créer un lien symbolique dans le répertoire `/etc/httpd/conf.dispatcher.d/enabled_farms/` Ils seront utilisés dans la configuration en cours d’exécution.

Les fichiers de fermes comportent des sous-inclusions basées sur [sections de niveau supérieur de la ferme de serveurs](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#defining-farms-farms) comme cache, clientheaders, filtres, rendus et vhosts.

Le `FILENAME_farm.any` Les fichiers contiennent des instructions pour chaque fichier en fonction de l’endroit où ils doivent être inclus dans le fichier de ferme.  Voici un exemple de syntaxe d’une `FILENAME_farm.any` comme référence appropriée :

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

Comme vous pouvez voir chaque section pour la ferme de serveurs weretail au lieu de disposer de toutes les syntaxes nécessaires, elle utilise plutôt une instruction d’inclusion.

Examinons la syntaxe de quelques-unes de ces inclusions pour avoir une idée de ce à quoi ressemblerait chaque sous-inclusion.

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any` :

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Comme vous pouvez le constater, il s’agit d’une nouvelle liste de noms de domaine séparés par des lignes qui doivent être rendus à partir de cette ferme de serveurs par rapport aux autres.

Regardons ensuite le `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Suivant -> Présentation du cache](./understanding-cache.md)