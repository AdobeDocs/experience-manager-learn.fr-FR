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
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 5%

---

# Présentation

L’utilisation d’images cliquables dans Forms peut créer une expérience utilisateur plus attrayante, intuitive et visuellement attrayante. Pour les besoins de cet article, nous avons utilisé SVG pour les images cliquables, car il offre plusieurs avantages, notamment en termes de flexibilité de conception, de performances et d’expérience utilisateur.
SVG peut être créé à l’aide d’Adobe Illustrator ou de n’importe quel outil en ligne gratuit. J&#39;ai utilisé la [carte des États-Unis d&#39;Amérique de ](https://simplemaps.com/resources/svg-us)simplemaps pour illustrer le cas d&#39;utilisation.

## Cas pratique d’utilisation de la carte Etats-Unis cliquable

La carte cliquable des Etats-Unis permet aux utilisateurs d’explorer les envois de formulaire spécifiques à l’état. Lorsqu’un utilisateur clique sur un état, les envois de cet état sont répertoriés, avec la possibilité d’ouvrir un envoi spécifique.
