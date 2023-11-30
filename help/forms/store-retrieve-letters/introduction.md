---
title: Enregistrer et reprendre des lettres
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
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 100%

---

# Présentation

Les communications interactives permettent aux personnes qui préparent des correspondances ad hoc d’enregistrer des correspondances partiellement terminées et de les récupérer ensuite pour continuer à travailler. AEM Forms vous fournit l’[interface du fournisseur de services](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). Le client ou la cliente doit mettre en œuvre cette interface pour obtenir la fonctionnalité Enregistrer et Reprendre.

Cet article utilise la base de données MySQL pour stocker les métadonnées de l’instance de lettre. Les données de lettre sont stockées sur le système de fichiers.

La vidéo suivante présente le cas d’utilisation :

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## Conditions préalables

Vous aurez besoin des éléments suivants pour mettre en œuvre la solution en fonction de vos besoins.

* Expérience professionnelle avec AEM Forms
* AEM Server 6.5 avec le module complémentaire Forms
* Connaissances relatives à la création de lots OSGi
