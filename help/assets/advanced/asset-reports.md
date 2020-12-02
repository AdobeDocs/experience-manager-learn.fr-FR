---
title: Rapports Ressources en AEM Assets
description: 'AEM Assets fournit un cadre de rapports au niveau de l’entreprise qui évolue pour les grands référentiels grâce à une expérience utilisateur intuitive. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# Rapports Ressources{#using-reports-in-aem-assets}

AEM Assets fournit un cadre de rapports au niveau de l’entreprise qui évolue pour les grands référentiels grâce à une expérience utilisateur intuitive.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Formules Microsoft Excel {#excel-formulas}

Les formules suivantes sont utilisées dans la vidéo pour générer le graphique Ressources par taille dans Microsoft Excel.

### Normalisation de la taille des ressources en octets {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### Nombre d&#39;actifs par taille {#asset-count-by-size}

#### Moins de 200 Ko {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 Ko à 500 Ko {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### Plus de 500 Ko {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## Ressources supplémentaires{#additional-resources}

Télécharger [Tous les fichiers Excel avec le graphique](./assets/asset-reports/all-assets.xlsx)
