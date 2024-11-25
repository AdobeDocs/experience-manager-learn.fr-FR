---
title: Ajout d’icônes personnalisées
description: Ajouter des icônes personnalisées aux onglets verticaux
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16418
exl-id: 20e44be0-5490-4414-9183-bb2d2a80bdf0
source-git-commit: faa859897b6b9fbb0acff02000611de216ddda3e
workflow-type: ht
source-wordcount: '676'
ht-degree: 100%

---

# Ajout d’icônes personnalisées

L’ajout d’icônes personnalisées aux onglets peut améliorer l’expérience d’utilisation et l’attrait visuel de plusieurs façons :

* Amélioration de l’utilisation : les icônes peuvent rapidement indiquer l’objectif de chaque onglet, ce qui facilite la recherche rapide des utilisateurs et utilisatrices. Les repères visuels tels que les icônes aident les utilisateurs et utilisatrices à naviguer de manière plus intuitive.

* Hiérarchie visuelle et concentration : les icônes créent une séparation plus distincte entre les onglets, ce qui améliore la hiérarchie visuelle. Les onglets importants sont ainsi distingués et l’attention des utilisateurs et utilisatrices est guidée plus efficacement.
En suivant cet article, vous devriez pouvoir placer les icônes comme illustré ci-dessous.

![Icônes](assets/icons.png)

## Conditions préalables

Pour suivre cet article, vous devez connaître Git, la création et le déploiement d’un projet AEM à l’aide de Cloud Manager, la configuration d’un pipeline front-end dans AEM Cloud Manager et un peu de CSS. Si vous ne connaissez pas les sujets mentionnés ci-dessus, suivez l’article [Utilisation de thèmes pour styliser les composants principaux](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/using-themes-in-core-components#rename-env-file-theme-folder).

## Ajouter des icônes au thème

Ouvrez le projet de thème dans le code Visual Studio ou tout autre éditeur de votre choix.
Ajoutez les icônes de votre choix au dossier d’images.
Les icônes marquées en rouge sont les nouvelles icônes ajoutées.
![new-icons](assets/newicons.png)

## Créer icon-map pour stocker les icônes

Créez l’élément icon-map dans le fichier _variable.scss. Le mappage SCSS $icon-map est un ensemble de paires clé-valeur, où chaque clé représente un nom d’icône (comme la maison, famille, etc.), et chaque valeur est le chemin d’accès au fichier image associé à cette icône.

![variable-scss](assets/variable_scss.png)

```css
$icon-map: (
    home: "./resources/images/home.png",
    family: "./resources/images/icons8-family-80.png",
    pdf: "./resources/images/pdf.png",
    income: "./resources/images/income.png",
    assets: "./resources/images/assets.png",
    cars: "./resources/images/cars.png"
);
```

## Ajouter un mixin

Ajoutez le code suivant au fichier _mixin.scss.

```css
@mixin add-icon-to-vertical-tab($image-url) {
  display: inline-flex;
  align-self: center;
  &::before {
    content: "";
    display:inline-block;
    background: url($image-url) left center / cover no-repeat;
    margin-right: 8px; /* Space between icon and text */
    height:40px;
    width:40px;
    vertical-align:middle;
    
  }
  
}
```

Le mixin add-icon-to-vertical-tab est conçu pour ajouter une icône personnalisée en regard du texte sur un onglet vertical. Il vous permet d’inclure facilement une image en tant qu’icône sur les onglets, de la positionner à côté du texte et de la mettre en forme pour garantir cohérence et alignement.

Répartition du mixin. Voici le rôle de chaque partie du mixin :

Paramètres :

* $image-url : URL de l’icône ou de l’image à afficher en regard du texte de l’onglet. La transmission de ce paramètre rend le mixin polyvalent, car il permet d’ajouter différentes icônes à différents onglets, selon les besoins.

* Styles appliqués :

   * display : inline-flex : fait de l’élément un conteneur flex, alignant tout contenu imbriqué (comme l’icône et le texte) horizontalement.
   * align-self : center : garantit que l’élément est centré verticalement dans son conteneur.
   * Pseudo-élément (::before) :
   * content : &quot;&quot; : initialise le pseudo-élément ::before, utilisé pour afficher l’icône en tant qu’image d’arrière-plan.
   * display : inline-block : définit le pseudo-élément sur inline-block, ce qui lui permet de se comporter comme une icône insérée avec le texte.
   * background : url($image-url) left center/cover no-repeat; : ajoute l’image d’arrière-plan à l’aide de l’URL fournie via $image-url. L’icône est alignée à gauche et centrée verticalement.

## Mettre à jour l’élément _verticaltabs.scss

Pour les besoins de l’article, j’ai créé une nouvelle classe css (cmp-verticaltabs—marketing) pour afficher les icônes des onglets. Dans cette nouvelle classe, nous étendons l’élément tab en ajoutant les icônes. La liste complète de la classe css est la suivante :

```css
.cmp-verticaltabs--marketing
{
  .cmp-verticaltabs
    {
      &__tab 
        {
          cursor:pointer;
            @each $name, $url in $icon-map {
            &[data-icon-name="#{$name}"]
              {
                  @include add-icon-to-vertical-tab($url);
              }
            }
        }
    }
}
```

## Modifier le composant verticaltabs

Copiez le fichier verticaltabs.html depuis ```/apps/core/fd/components/form/verticaltabs/v1/verticaltabs/verticaltabs.html``` et collez-le sous le composant verticaltabs de votre projet. Ajoutez la ligne suivante ```data-icon-name="${tab.name}"``` au fichier copié sous le rôle li, comme illustré dans l’image ci-dessous.
![data-icon](assets/data-icons.png)
Nous définissons un attribut de données personnalisé appelé data-icon-name avec la valeur du nom de l’onglet. Si le nom de l’onglet correspond à un nom d’image dans la carte de l’icône, l’image correspondante est associée à l’onglet.



## Tester le code

Déployez le composant verticaltabs mis à jour sur votre instance cloud.
Déployez le thème mis à jour à l’aide du pipeline front-end.
Créez une variation de style pour les composants de l’onglet vertical comme illustré ci-dessous
![style-variation](assets/verticaltab-style-variation.png)
Nous avons créé une variation de style appelée Marketing qui est associée à la classe css _**cmp-verticaltabs—marketing**_.
Créez un formulaire adaptatif avec un composant d’onglet vertical. Associez le composant d’onglet vertical à la variation de style marketing.
Ajoutez quelques onglets aux onglets verticaux et nommez-les pour qu’ils correspondent aux images définies dans la carte des icônes, telles que home,family.
![home-icon](assets/tab-name.png)

Prévisualisez le formulaire. Les icônes correspondantes doivent s’afficher dans l’onglet.
