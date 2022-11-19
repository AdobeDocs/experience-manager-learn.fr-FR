---
title: Présentation de "Dispatcher"
description: Comprendre ce qu’est réellement un Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# Présentation de &quot;Dispatcher&quot;

[Table des matières](./overview.md)

Commençons par la description de base de ce qui implique un Dispatcher AEM.

## Serveur web Apache

Commencez par une installation Apache Web Server de base sur un serveur Linux.

Explication de base des actions d’un serveur Apache :

- Suit des règles simples pour diffuser des fichiers sur les protocoles HTTP(s) à partir de son répertoire de document statique (`DocumentRoot`)
- Fichiers stockés dans un emplacement par défaut (`/var/www/html`) sont mis en correspondance sur les demandes et générés dans le navigateur du client demandeur.




## AEM fichier de module spécifique (`mod_dispatcher.so`)

Ajoutez ensuite un module externe au serveur web Apache appelé module de Dispatcher.

Explication de base de ce que fait le module Adobe AEM Dispatcher :

- Augmente le gestionnaire de fichiers par défaut
- Filtre les mauvaises requêtes / Protège AEM ventre doux/points de terminaison
- Équilibres de charge si plusieurs moteurs de rendu sont présents
- Permet un répertoire de cache actif/prend en charge le vidage des fichiers en stagnation
- Il s’agit de la porte d’entrée de toutes les installations AMS et il diffuse des sites web et des ressources sur le navigateur du client.
- Il met en cache les demandes de réutilisation à un rythme beaucoup plus rapide qu’un serveur AEM ne peut effectuer seul.
- Beaucoup plus...

## Workflow de trafic web

Comprendre les éléments qui sont installés ensemble pour créer un serveur Dispatcher de base nous permet de vous faire comprendre le processus de trafic web de base pour une configuration des services Adobe Manager.
Cela devrait vous aider à comprendre le rôle qu’il joue dans la chaîne des systèmes qui diffusent du contenu aux visiteurs de votre contenu AEM.

<b>Diffusion de contenu déjà mis en cache</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Diffusion de contenu frais à partir d’AEM</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>Publication/modifications de contenu</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Next -> Basic File Layout](./basic-file-layout.md)