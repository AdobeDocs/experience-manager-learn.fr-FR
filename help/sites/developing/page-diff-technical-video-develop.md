---
title: Développement pour la différence de page en AEM Sites
description: Cette vidéo montre comment fournir des styles personnalisés pour la fonctionnalité Différence de page des sites AEM.
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 6%

---


# Développement pour la différence de page {#developing-for-page-difference}

Cette vidéo montre comment fournir des styles personnalisés pour la fonctionnalité Différence de page des sites AEM.

## Personnalisation des styles de différences de page {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Cette vidéo ajoute une page CSS personnalisée à la bibliothèque cliente we.Retail, où ces modifications doivent être apportées au projet AEM Sites du personnaliseur ; dans l’exemple de code ci-dessous : `my-project`.

aem différence de page obtient le fichier CSS OOTB via un chargement direct de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

En raison de ce chargement direct de CSS plutôt que d’utiliser une catégorie de bibliothèque cliente, nous devons trouver un autre point d’injection pour les styles personnalisés, et ce point d’injection personnalisé est la bibliothèque cliente de création du projet.

Cela a pour avantage de permettre que ces remplacements de style personnalisé soient spécifiques au client.

### Préparer la bibliothèque cliente de création {#prepare-the-authoring-clientlib}

Assurez l&#39;existence d&#39;une bibliothèque cliente `authoring` pour votre projet à l&#39;adresse `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fournir le CSS personnalisé {#provide-the-custom-css}

Ajoutez au `authoring` clientlib du projet `css.txt` qui pointe vers le fichier less qui fournira les styles de remplacement. [](https://lesscss.org/) Lessis a préféré en raison de ses nombreuses fonctionnalités pratiques, dont l’encapsulation de classe, qui est exploitée dans cet exemple.

```shell
base=./css

htmldiff.less
```

Créez le fichier `less` qui contient les remplacements de style à `/apps/my-project/clientlibs/authoring/css/htmldiff.less` et fournissez les styles de remplacement nécessaires.

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

### Inclure le fichier CSS clientlib de création via le composant de page {#include-the-authoring-clientlib-css-via-the-page-component}

Insérez la catégorie clientlibs de création dans la page de base `/apps/my-project/components/structure/page/customheaderlibs.html` du projet directement avant la balise `</head>` pour vous assurer que les styles sont chargés.

Ces styles doivent être limités aux modes [!UICONTROL Edit] et [!UICONTROL prévisualisation] WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

Le résultat final d’une page diff avec les styles ci-dessus appliqués ressemblerait à ceci (HTML ajouté et Composant modifié).

![Différence de page](assets/page-diff.png)

## Ressources supplémentaires {#additional-resources}

* [Téléchargement du site d’exemple we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Utilisation des bibliothèques clientes AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Documentation CSS less](https://lesscss.org/)
