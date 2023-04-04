---
title: Développement pour la différence de page dans AEM Sites
description: Cette vidéo montre comment fournir des styles personnalisés pour la fonctionnalité Différence de page d’AEM Sites.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 6%

---

# Développement pour la différence de page {#developing-for-page-difference}

Cette vidéo montre comment fournir des styles personnalisés pour la fonctionnalité Différence de page d’AEM Sites.

## Personnalisation des styles de différence de page {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>Cette vidéo ajoute une page CSS personnalisée à la bibliothèque cliente we.Retail, où ces modifications doivent être apportées au projet AEM Sites du personnalisateur. dans l’exemple de code ci-dessous : `my-project`.

AEM différence de page obtient le fichier CSS prêt à l’emploi via un chargement direct de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

En raison de ce chargement direct de CSS plutôt que d’utiliser une catégorie de bibliothèque cliente, nous devons trouver un autre point d’injection pour les styles personnalisés, et ce point d’injection personnalisé est la bibliothèque cliente de création du projet.

Cela a l’avantage de permettre à ces remplacements de style personnalisés d’être spécifiques au client.

### Préparation de la bibliothèque cliente de création {#prepare-the-authoring-clientlib}

Assurez-vous qu’une `authoring` clientlib pour votre projet à l’adresse `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fournir le CSS personnalisé {#provide-the-custom-css}

Ajouter à la `authoring` clientlib a `css.txt` qui pointe vers le fichier less qui fournira les styles de remplacement. [Inférieur](https://lesscss.org/) est préférable en raison de ses nombreuses fonctionnalités pratiques, notamment l’encapsulage de classe utilisé dans cet exemple.

```shell
base=./css

htmldiff.less
```

Créez le `less` qui contient le style remplace le `/apps/my-project/clientlibs/authoring/css/htmldiff.less`, et fournissez les styles de recouvrement selon les besoins.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### Inclure le CSS clientlib de création via le composant de page {#include-the-authoring-clientlib-css-via-the-page-component}

Inclure la catégorie clientlibs de création dans la page de base du projet `/apps/my-project/components/structure/page/customheaderlibs.html` directement avant l’événement `</head>` pour vous assurer que les styles sont chargés.

Ces styles doivent être limités à [!UICONTROL Modifier] et [!UICONTROL aperçu] Modes WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Le résultat final d’une page diff avec les styles ci-dessus appliqués ressemblerait à ceci (HTML ajouté et composant modifié).

![Différence entre les pages](assets/page-diff.png)

## Ressources supplémentaires {#additional-resources}

* [Téléchargez l’exemple de site we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Utilisation des bibliothèques clientes AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Documentation CSS réduite](https://lesscss.org/)
