---
title: Créer un composant de workflow pour enregistrer les pièces jointes de formulaire dans le système de fichiers
description: Écrire des pièces jointes de formulaire adaptatif dans un système de fichiers à l’aide d’un composant de workflow personnalisé
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '364'
ht-degree: 100%

---

# Composant de workflow personnalisé

Ce tutoriel est destiné aux clientes et clients d’AEM Forms qui doivent créer un composant de workflow personnalisé. Le composant de workflow sera configuré pour exécuter le code écrit à l’étape précédente. Le composant de workflow peut spécifier des arguments de processus au code. Dans cet article, nous allons explorer le composant de workflow associé au code.


[Télécharger le composant de workflow personnalisé](assets/saveFiles.zip)
Importer le composant de workflow [à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp)

Le composant de workflow personnalisé se trouve dans /apps/AEMFormsDemoListings/workflowcomponent/SaveFiles.

Sélectionnez le nœud SaveFiles et examinez ses propriétés.

**componentGroup** : la valeur de cette propriété détermine la catégorie du composant de workflow.

**jcr:Title** : il s’agit du titre du composant de workflow.

**sling:resourceSuperType** : la valeur de cette propriété détermine l’héritage de ce composant. Dans ce cas, nous héritons à partir du composant de processus.


![component-properties](assets/component-properties1.png)

## cq:dialog

Les boîtes de dialogue permettent à l’auteur ou à l’autrice d’interagir avec le composant. cq:dialog se trouve sous le nœud SaveFiles.
![cq-dialog](assets/cq-dialog.png)

Les nœuds situés sous le nœud des éléments (items) représentent les onglets du composant par le biais desquels les personnes chargées de créer du contenu interagiront avec le composant. Les onglets communs et de de processus sont masqués. Les onglets communs et d’arguments sont visibles.

Les arguments de processus pour le processus se trouvent sous le nœud processargs.

![process-args](assets/process-arguments.png)

La personne chargée de créer du contenu spécifie les arguments comme illustré dans la capture d’écran ci-dessous.
![workflow-component](assets/custom-workflow-component.png)

Les valeurs sont stockées en tant que propriétés du nœud de métadonnées. Par exemple, la valeur **c:\formsattachments** sera stockée dans la propriété saveToLocation du nœud de métadonnées.
![save-location](assets/save-to-location.png)

## cq:editConfig

cq:EditConfig est simplement un nœud ave le type principal cq:EditConfig et le nom cq:editConfig sous la racine du composant.
Le comportement de modification d’un composant est configuré en ajoutant un nœud cq:editConfig de type cq:EditConfig sous le nœud du composant (de type cq:Component).

![edit-config](assets/cq-edit-config.png)

cq:formParameters (type de nœud nt:unstructured) : définit des paramètres supplémentaires qui sont ajoutés au formulaire de boîte de dialogue.


Notez les propriétés du nœud cq:formParameters.
![from-parameters-properties](assets/form-parameters-properties.png)

La valeur de la propriété PROCESS indique le code Java qui sera associé au composant de workflow.
