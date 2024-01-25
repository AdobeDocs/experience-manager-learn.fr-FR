---
title: Journaux courants du Dispatcher AEM
description: Consultez les entrées de journal courantes de Dispatcher, apprenez ce qu’elles signifient et comment les traiter.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 275
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 100%

---

# Journaux courants

[Table des matières](./overview.md)

[&lt;- Précédent : URL de redirection](./disp-vanity-url.md)

## Vue d’ensemble

Le document décrit les entrées de journal courantes que vous verrez, ce qu’elles signifient et comment les traiter.

## Avertissement GLOB

Exemple d’entrée de journal :

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Depuis le module Dispatcher 4.2.x, les personnes ont été incitées à ne plus utiliser le type de correspondance suivant dans leur fichier de filtres :

```
/0041 { /type "allow" /glob "* *.css *"   }
```

Il est préférable d’utiliser la nouvelle syntaxe :

```
/0041 { /type "allow" /url "*.css" }
```

Ou mieux encore, de ne pas utiliser la correspondance avec les caractères génériques :

```
/0041 { /type "allow" /extension "css" }
```

L’une ou l’autre des méthodes proposées permet d’éliminer ce message d’erreur des journaux.

## Filtres de refus


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
ces entrées ne s’affichent pas toujours même si des refus se produisent si le niveau de journalisation est trop bas. Définissez-le sur Info ou Déboguer pour vous assurer que les filtres refusent les requêtes.
</div>

Exemple d’entrée de journal :

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

ou :

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>Attention :</b>

Il faut comprendre que les règles de Dispatcher ont été définies pour filtrer cette requête. Dans ce cas, la page qui a tenté d’être visitée a été refusée intentionnellement et nous ne ferons rien à ce propos.
</div>

Si votre journal ressemble à cette entrée :

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Cela nous indique que notre fichier de conception `.eot` est bloqué et nous devons résoudre ce problème.
Nous devons donc examiner notre fichier de filtres et ajouter la ligne suivante pour autoriser les fichiers `.eot` à passer.

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

Cela permet au fichier de passer et d’empêcher la journalisation.
Si vous souhaitez voir ce qui est filtré, vous pouvez exécuter cette commande sur votre fichier journal :

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Délais d’expiration du rendu

Exemple d’entrée de journal pour le délai d’expiration de socket :

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Cela se produit lorsque l’adresse IP incorrecte est configurée dans la section des rendus de votre batterie de serveurs. Il s’agit de cela, ou l’instance AEM a cessé de répondre ou d’écouter et Dispatcher ne peut pas y accéder.

Vérifiez les règles de votre pare-feu et que l’instance AEM fonctionne correctement.

Exemples d’entrées de journal pour le délai d’expiration de la passerelle :

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Cela signifie que l’instance AEM avait un socket ouvert qu’elle pouvait atteindre et que le délai de réponse a expiré. Cela signifie que votre instance AEM était trop lente ou qu’elle n’est pas saine et que Dispatcher a atteint les paramètres de délai d’expiration configurés dans la section de rendu de la batterie de serveurs. Augmentez le paramètre de délai d’expiration ou assurez-vous que votre instance AEM est saine.

## Niveau de mise en cache

Exemple d’entrée de journal :

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Cela signifie que l’on mesure la récupération au niveau de rendu par rapport au cache. Si vous souhaitez atteindre plus de 80 % du cache, suivez les instructions décrites [ici](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den?lang=fr) :

Pour rendre ce nombre le plus élevé possible.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Remarque :</b>
même si le fichier de batterie contient vos paramètres de cache pour tout mettre en cache, vous purgez peut-être trop souvent, ou de manière trop agressive, ce qui peut entraîner un taux d’accès au cache inférieur.
</div>

## Répertoire manquant

Exemple d’entrée de journal :

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Cela s’affiche généralement lorsqu’un élément est récupéré alors qu’un effacement du cache est en cours.

Soit le répertoire racine du document contient des autorisations incorrectes, soit le contexte de fichier SELinux sur le dossier est incorrect, ce qui empêche Apache de créer les sous-répertoires nécessaires.

Pour les problèmes d’autorisation, examinez les autorisations de la racine du document et assurez-vous qu’elles ressemblent à ce qui suit :

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## URL de redirection introuvable

Exemple d’entrée de journal :

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Cette erreur se produit lorsque vous avez configuré Dispatcher pour qu’il utilise le filtre automatique dynamique afin d’autoriser les URL de redirection, mais que vous n’avez pas terminé la configuration en installant le package sur le moteur de rendu AEM.

Pour corriger ce problème, installez le pack de fonctionnalités d’URL de redirection sur l’instance AEM et autorisez l’utilisateur ou l’utilisatrice anonyme à le lire. Plus de détails [ici](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den?lang=fr).

Une configuration fonctionnelle d’URL de redirection ressemble à ceci :

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Batterie manquante

Exemple d’entrée de journal :

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Cette erreur indique que, parmi tous les fichiers de batterie disponibles dans `/etc/httpd/conf.dispatcher.d/enabled_farms/`, ils n’ont pas pu trouver d’entrée correspondante à partir de la section `/virtualhost`.

Les fichiers de batterie correspondent au trafic en fonction du nom de domaine ou du chemin d’accès dans lequel la requête est arrivée. Il utilise la correspondance glob. Une absence de correspondance signifie que vous n’avez pas configuré correctement votre batterie, vous avez commis une erreur en saisissant l’entrée dans la batterie ou l’entrée est manquante. Lorsque la batterie ne correspond à aucune entrée, elle passe finalement par défaut sur la dernière batterie incluse dans la pile des fichiers de batterie inclus. Dans cet exemple, c’est `999_ams_publish_farm.any` qui prend le nom générique de publishfarm (batterie de publication).

Voici un exemple de fichier de batterie `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` qui a été réduit pour mettre en évidence les parties pertinentes.

## Élément diffusé à partir de

Exemple d’entrée de journal :

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

La page a été récupérée via la méthode HTTP GET pour le contenu `/content/we-retail/us/en.html`. L’opération a pris 24 034 millisecondes. La partie à laquelle nous voulions prêter attention est à la fin `publishfarm/0`. Vous verrez qu’elle a ciblé `publishfarm` et que cela a correspondu. La requête a été récupérée à partir du rendu 0. Cela signifie que cette page a dû être demandée auprès d’AEM, puis mise en cache. Demandons à nouveau cette page et voyons ce qui se passe dans le journal.

[Suivant -> Fichiers en lecture seule](./immutable-files.md)
