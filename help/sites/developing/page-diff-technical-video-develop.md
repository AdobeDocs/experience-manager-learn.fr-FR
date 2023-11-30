---
title: Développer pour la différence de page dans AEM Sites
description: Cette vidéo montre comment fournir des styles personnalisés pour la fonctionnalité Différence de page d’AEM Sites.
feature: Authoring
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 100%

---

# Développer pour la différence de page {#developing-for-page-difference}

Cette vidéo montre comment fournir des styles personnalisés pour la fonctionnalité Différence de page d’AEM Sites.

## Personnalisation des styles de différence de page {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>Cette vidéo ajoute un CSS personnalisé à la bibliothèque cliente we.Retail, où ces modifications doivent être apportées au projet AEM Sites du personnalisateur ou de la personnalisatrice dans l’exemple de code ci-dessous : `my-project`.

La différence de page AEM obtient le CSS prêt à l’emploi via un chargement direct de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

En raison de ce chargement direct de CSS plutôt que d’utiliser une catégorie de bibliothèque cliente, nous devons trouver un autre point d’injection pour les styles personnalisés, et ce point d’injection personnalisé est la bibliothèque cliente de création du projet.

Cela a l’avantage de permettre à ces remplacements de style personnalisés d’être spécifiques au client ou à la cliente.

### Préparer la bibliothèque cliente de création {#prepare-the-authoring-clientlib}

Assurez-vous de l’existence d’une bibliothèque cliente `authoring` pour votre projet dans `/apps/my-project/clientlib/authoring.`.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fournir le CSS personnalisé {#provide-the-custom-css}

Ajoutez à la bibliothèque cliente `authoring` du projet un `css.txt` qui pointe vers le fichier less qui fournira les styles de remplacement. [Less](https://lesscss.org/) est préférable en raison de ses nombreuses fonctionnalités pratiques, notamment l’encapsulage de classe utilisé dans cet exemple.

```shell
base=./css

htmldiff.less
```

Créez le fichier `less` qui contient les remplacements de style sur `/apps/my-project/clientlibs/authoring/css/htmldiff.less`, puis fournissez les styles de remplacement selon les besoins.

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

### Inclure le CSS de bibliothèque cliente de création via le composant de page {#include-the-authoring-clientlib-css-via-the-page-component}

Incluez la catégorie de bibliothèque cliente de création dans la page de base du projet `/apps/my-project/components/structure/page/customheaderlibs.html` directement avant la balise `</head>` pour vous assurer que les styles sont chargés.

Ces styles doivent être limités aux modes WCM [!UICONTROL Modifier] et [!UICONTROL Aperçu].

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Le résultat final d’une page différenciée avec les styles ci-dessus appliqués ressemblerait à ceci (HTML ajouté et composant modifié).

![Différence de page.](assets/page-diff.png)

## Ressources supplémentaires {#additional-resources}

* [Télécharger l’exemple de site we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Utiliser des bibliothèques clientes AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Documentation CSS Less](https://lesscss.org/)
