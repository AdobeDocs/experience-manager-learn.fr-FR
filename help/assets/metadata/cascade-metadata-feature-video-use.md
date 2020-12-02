---
title: Utilisation de métadonnées en cascade en AEM Assets
seo-title: Utilisation de métadonnées en cascade en AEM Assets
description: La gestion avancée des métadonnées permet aux utilisateurs de créer des règles de champ en cascade pour créer des relations contextuelles entre les métadonnées en AEM Assets. La vidéo ci-dessous présente de nouvelles règles dynamiques pour les exigences de champ, la visibilité et les choix contextuels. La vidéo détaille également les étapes nécessaires pour qu’un administrateur applique ces règles à un schéma de métadonnées personnalisé.
seo-description: La gestion avancée des métadonnées permet aux utilisateurs de créer des règles de champ en cascade pour créer des relations contextuelles entre les métadonnées en AEM Assets. La vidéo ci-dessous présente de nouvelles règles dynamiques pour les exigences de champ, la visibilité et les choix contextuels. La vidéo détaille également les étapes nécessaires pour qu’un administrateur applique ces règles à un schéma de métadonnées personnalisé.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# Utilisation de métadonnées en cascade en AEM Assets{#using-cascading-metadata-in-aem-assets}

La gestion avancée des métadonnées permet aux utilisateurs de créer des règles de champ en cascade pour créer des relations contextuelles entre les métadonnées en AEM Assets. La vidéo ci-dessous présente de nouvelles règles dynamiques pour les exigences de champ, la visibilité et les choix contextuels. La vidéo détaille également les étapes nécessaires pour qu’un administrateur applique ces règles à un schéma de métadonnées personnalisé.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

Il existe trois jeux de règles dynamiques qui peuvent être activés pour un champ de métadonnées donné :

1. **Exigence**  : un champ peut être marqué de manière dynamique comme requis pour être basé sur la valeur d’un autre champ déroulant.

2. **Visibilité**  : peuvent toujours être visibles ou uniquement visibles en fonction de la valeur d’un autre champ déroulant.

3. **Choix**  : (applicable uniquement aux champs déroulants) filtrez les choix affichés à l’utilisateur en fonction de la valeur actuellement sélectionnée d’un autre champ déroulant.

>[!NOTE]
>
>Les règles en cascade peuvent SEULEMENT être créées en fonction des valeurs d’un champ déroulant. Il est possible d’appliquer les trois jeux de règles au même champ de métadonnées mais, en règle générale, il est recommandé de rendre chaque jeu de règles dépendant de la même liste déroulante de métadonnées.

Télécharger [Package de métadonnées personnalisées](assets/cascade-metadata-values-001.zip)

## Ressources supplémentaires{#additional-resources}

Schéma de métadonnées personnalisées créé dans : `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. Le package AEM ci-dessous applique un schéma personnalisé au dossier : `/content/dam/we-retail/en/activities` :

