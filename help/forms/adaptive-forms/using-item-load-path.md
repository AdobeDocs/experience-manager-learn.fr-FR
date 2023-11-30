---
title: Utiliser le chemin de chargement des éléments pour remplir la liste déroulante
description: Configurer et remplir une liste déroulante pour lire les valeurs d’un nœud CRX
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 100%

---

# Propriété de chargement des éléments dans AEM Forms

Configurez et renseignez la liste déroulante à l’aide de la propriété du chemin de chargement des éléments.
Le champ Chemin de chargement des éléments permet à une personne créatrice de fournir une URL à partir de laquelle elle charge les options disponibles dans une liste déroulante.
Pour créer un tel nœud dans CRX, procédez comme suit :
* connectez-vous à CRX ;
* créez un nœud appelé assets (vous pouvez nommer ce nœud selon vos besoins) de type sling:folder sous content ;
* enregistrez ;
* cliquez sur le nœud de ressources nouvellement créé et définissez ses propriétés comme illustré ci-dessous.
* Vous devez créer une propriété de type Chaîne appelée assettypes (vous pouvez la nommer selon vos besoins). Assurez-vous que la propriété est composée de plusieurs valeurs. Indiquez les valeurs que vous souhaitez et enregistrez.
  ![item-load-path](assets/item-load-path-crx.png)

Pour charger ces valeurs dans votre liste déroulante, indiquez le chemin suivant dans la propriété du chemin de chargement d’éléments **/content/assets/assettypes**.

L’exemple de package peut être [téléchargé ici](assets/item-load-path-package.zip).
