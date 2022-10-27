---
title: Utilisation du chemin de chargement des éléments pour remplir la liste déroulante
description: Configuration et remplissage d’une liste déroulante pour lire les valeurs d’un noeud crx
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Propriété de chargement d’élément dans AEM Forms

Configurez et renseignez la liste déroulante à l’aide de la propriété item load path .
Le champ Chemin de chargement de l’élément permet à un auteur de fournir une URL à partir de laquelle il charge les options disponibles dans une liste déroulante.
Pour créer un tel noeud dans crx, procédez comme suit :
* Connexion à crx
* Créez un noeud appelé assets (vous pouvez nommer ce noeud selon vos besoins) de type sling:folder sous content.
* Enregistrer
* Cliquez sur le noeud de ressources nouvellement créé et définissez ses propriétés comme illustré ci-dessous.
* Vous devez créer une propriété de type Chaîne appelée assettypes (vous pouvez la nommer selon vos besoins). Assurez-vous que la propriété est composée de plusieurs valeurs. Indiquez les valeurs que vous souhaitez et enregistrez.
   ![item-load-path](assets/item-load-path-crx.png)

Pour charger ces valeurs dans votre liste déroulante, indiquez le chemin suivant dans la propriété de chemin de chargement d’élément .  **/content/assets/assettypes**

L’exemple de package peut être [téléchargé ici](assets/item-load-path-package.zip)
