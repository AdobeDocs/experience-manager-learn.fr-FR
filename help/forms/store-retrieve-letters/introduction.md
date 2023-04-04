---
title: Enregistrement et reprise de lettres
seo-title: Save and resume letters
description: Découvrez comment enregistrer et récupérer des brouillons de lettres
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# Présentation

Les communications interactives permettent aux agents qui préparent des correspondances ad hoc d’enregistrer des correspondances partiellement terminées et de récupérer les mêmes correspondances pour continuer à fonctionner. AEM Forms vous fournit les [Interface du fournisseur de services](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Le client doit mettre en oeuvre cette interface pour obtenir la fonctionnalité Enregistrer et reprendre .

Cet article utilise la base de données MySQL pour stocker les métadonnées de l’instance de lettre. Les données de lettre sont stockées sur le système de fichiers.

La vidéo suivante présente le cas pratique :

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Conditions préalables

Vous aurez besoin des éléments suivants pour mettre en oeuvre la solution en fonction de vos besoins.

* Utilisation d’AEM Forms
* AEM Server 6.5 avec Forms Add on
* Être familier avec la création de lots OSGI
