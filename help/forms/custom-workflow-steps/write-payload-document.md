---
title: Écrire le document de charge utile dans le système de fichiers
description: Étape de processus personnalisée pour ajouter au système de fichiers le document d’écriture résidant sous le dossier de charge utile
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-9859
source-git-commit: 160471fdc34439da6c312d65b252eaa941b7c7a2
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Écrire le document dans le système de fichiers

Le cas d’utilisation courant consiste à écrire les documents générés dans le workflow dans le système de fichiers.
Cette étape personnalisée du processus de workflow facilite l’écriture des documents de workflow dans le système de fichiers.
Le processus personnalisé utilise les arguments séparés par des virgules suivants :

```java
ChangeBeneficiary.pdf,c:\confirmation
```

Le premier argument est le nom du document que vous souhaitez enregistrer dans le système de fichiers. Le deuxième argument correspond à l’emplacement du dossier dans lequel vous souhaitez enregistrer le document. Par exemple, dans le cas d’utilisation ci-dessus, le document sera écrit sur c:\confirmation\ChangeBeneficiary.pdf

La capture d’écran suivante montre les arguments que vous devez transmettre à l’étape de processus personnalisée.
![write-payload-file-system](assets/write-payload-file-system.png)

[Le lot personnalisé peut être téléchargé ici](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)