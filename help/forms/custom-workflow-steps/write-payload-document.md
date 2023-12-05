---
title: Écrire le document de payload dans le système de fichiers
description: Il s’agit d’une étape de processus personnalisée permettant d’écrire sur le système de fichiers le document résidant sous le dossier de payload.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
exl-id: bab7c403-ba42-4a91-8c86-90b43ca6026c
last-substantial-update: 2020-07-07T00:00:00Z
duration: 43
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 100%

---

# Écrire le document dans le système de fichiers

Un cas d’utilisation courant consiste à écrire les documents générés dans le workflow dans le système de fichiers.
Cette étape de workflow personnalisée permet de faciliter l’écriture des documents de workflow dans le système de fichiers.
Cette étape utilise les arguments suivants, séparés par des virgules :

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Le premier argument est le nom du document que vous souhaitez enregistrer dans le système de fichiers. Le deuxième argument correspond à l’emplacement du dossier dans lequel vous souhaitez enregistrer le document. Par exemple, dans le cas d’utilisation ci-dessus, le document est enregistré sous `c:\confirmation\ChangeBeneficiary.pdf`.

La copie d’écran suivante montre les arguments que vous devez transmettre à l’étape de workflow personnalisée.
![write-payload-file-system](assets/write-payload-file-system.png)

[Le lot personnalisé peut être téléchargé ici.](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
