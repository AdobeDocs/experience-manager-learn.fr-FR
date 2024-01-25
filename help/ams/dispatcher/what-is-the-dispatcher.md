---
title: Qu’est-ce que le Dispatcher ?
description: Comprendre ce qu’est un Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 85
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Qu’est-ce que le Dispatcher ?

[Table des matières](./overview.md)

Commençons par la description de base de ce qu’implique un Dispatcher AEM.

## Serveur web Apache

Commencez par une installation de serveur web Apache de base sur un serveur Linux.

Explication de base des actions d’un serveur Apache :

- Il suit des règles simples pour diffuser des fichiers sur les protocoles HTTP(s) à partir de son répertoire de document statique (`DocumentRoot`).
- Les fichiers stockés dans un emplacement par défaut (`/var/www/html`) sont mis en correspondance avec les demandes et générés dans le navigateur du client demandeur.




## Fichier de module spécifique AEM (`mod_dispatcher.so`)

Ajoutez ensuite un plug-in au serveur web Apache appelé module Dispatcher.

Explication de base des actions du module de Dispatcher Adobe AEM :

- Il augmente le gestionnaire de fichiers par défaut.
- Il filtre les mauvaises requêtes et protège les faiblesses et points d’entrée AEM vulnérables.
- Il équilibre la charge si plusieurs moteurs de rendu sont présents.
- Il permet un répertoire de cache actif/prend en charge le vidage des fichiers inactifs.
- Il constitue la porte d’entrée de toutes les installations AMS et il diffuse des sites web et des ressources sur le navigateur du client.
- Il met en cache les demandes de rediffusion à un rythme beaucoup plus rapide qu’un serveur AEM ne pourrait le faire seul.
- Bien plus...

## Workflow de trafic web

Comprendre les éléments installés ensemble pour créer un serveur Dispatcher de base nous permet de vous faire comprendre le workflow de trafic web de base pour une configuration d’Adobe Managed Services.
Cela devrait vous aider à comprendre le rôle qu’il joue dans la chaîne des systèmes qui diffusent du contenu aux visiteurs et visiteuses de votre contenu AEM.

<b>Diffuser du contenu déjà mis en cache</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Diffuser du nouveau contenu à partir d’AEM</b>

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

<b>Publier/modifier du contenu</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Suivant -> Disposition de fichier de base](./basic-file-layout.md)
