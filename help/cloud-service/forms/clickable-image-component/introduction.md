---
title: Création d’un composant d’image cliquable
description: Création d’un composant d’image cliquable dans AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: ht
source-wordcount: '132'
ht-degree: 100%

---

# Présentation

L’utilisation d’images cliquables dans Forms peut créer une expérience client plus interactive, intuitive et visuellement attrayante. Pour les besoins de cet article, nous avons utilisé le format SVG pour les images cliquables, car il offre plusieurs avantages, notamment en termes de flexibilité de conception, de performances et d’expérience client.
Le format SVG peut être créé à l’aide d’Adobe Illustrator ou de n’importe quel outil en ligne gratuit. J’ai utilisé la [carte des États-Unis d’Amérique de](https://simplemaps.com/resources/svg-us) simplemaps pour illustrer le cas d’utilisation.

## Cas d’utilisation de la carte cliquable des États-Unis

La carte cliquable des États-Unis permet aux utilisateurs et aux utilisatrices d’explorer les envois de formulaire spécifiques à un État. Lorsqu’une personne clique sur un État, les envois de cet État sont répertoriés, avec la possibilité d’ouvrir un envoi spécifique.
