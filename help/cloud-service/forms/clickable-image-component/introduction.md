---
title: Création d’un composant Image cliquable
description: Création d’un composant d’image cliquable dans AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 5%

---

# Présentation

L’utilisation d’images cliquables dans Forms peut créer une expérience utilisateur plus attrayante, intuitive et visuellement attrayante. Pour les besoins de cet article, nous avons utilisé SVG pour les images cliquables, car il offre plusieurs avantages, notamment en termes de flexibilité de conception, de performances et d’expérience utilisateur.
SVG peut être créé à l’aide d’Adobe Illustrator ou de n’importe quel outil en ligne gratuit. J&#39;ai utilisé la [carte des États-Unis d&#39;Amérique de ](https://simplemaps.com/resources/svg-us)simplemaps pour illustrer le cas d&#39;utilisation.

## Cas pratique d’utilisation de la carte Etats-Unis cliquable

La carte cliquable des Etats-Unis permet aux utilisateurs d’explorer les envois de formulaire spécifiques à l’état. Lorsqu’un utilisateur clique sur un état, les envois de cet état sont répertoriés, avec la possibilité d’ouvrir un envoi spécifique.
