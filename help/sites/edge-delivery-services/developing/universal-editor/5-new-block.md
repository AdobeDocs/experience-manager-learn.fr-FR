---
title: Créer un bloc
description: Créez un bloc pour un site web Edge Delivery Services modifiable avec l’éditeur universel.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1767'
ht-degree: 100%

---

# Créer un bloc

Ce chapitre décrit le processus de création d’un bloc de teaser modifiable pour un site web Edge Delivery Services à l’aide de l’éditeur universel.

![Nouveau bloc de teaser](./assets//5-new-block/teaser-block.png)

Le bloc, nommé `teaser`, présente les éléments suivants :

- **Image** : image visuellement attrayante.
- **Contenu texte** :
   - **Titre** : titre attrayant pour attirer l’attention.
   - **Corps de texte** : contenu descriptif fournissant du contexte ou des détails, y compris des conditions générales facultatives.
   - **Bouton d’appel à l’action (CTA)** : lien conçu pour inciter la personne à interagir et à s’engager davantage.

Le contenu du bloc `teaser` est modifiable dans l’éditeur universel, ce qui garantit sa facilité d’utilisation et de réutilisation sur l’ensemble du site web.

Notez que le bloc `teaser` est similaire au bloc `hero` du modèle standard. Par conséquent, le bloc `teaser` n’est destiné qu’à servir d’exemple simple pour illustrer des concepts de développement.

## Créer une branche Git

Pour maintenir un workflow propre et organisé, créez une branche pour chaque tâche de développement spécifique. Cela permet d’éviter les problèmes de déploiement de code incomplet ou non testé en production.

1. **Commencez à partir de la branche principale** : le fait de travailler avec le code de production le plus récent garantit une base solide.
2. **Récupérez les modifications à distance** : la récupération des dernières mises à jour de GitHub garantit que le code le plus récent est disponible avant de commencer le développement.
   - Exemple : après avoir fusionné des modifications de la branche `wknd-styles` dans `main`, récupérez les dernières mises à jour.
3. **Créez une branche** :

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

Une fois la branche `teaser` créée, vous êtes en mesure de commencer à développer le bloc de teaser.

## Dossier de bloc

Créez un dossier nommé `teaser` dans le répertoire `blocks` du projet. Ce dossier contient les fichiers JSON, CSS et JavaScript du bloc, ce qui organise les fichiers du bloc en un seul emplacement :

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

Le nom du dossier de blocs fait office d’identifiant du bloc et est utilisé pour référencer le bloc tout au long de son développement.

## JSON de bloc

Le JSON de bloc définit trois aspects clés du bloc :

- **Définition** : enregistre le bloc en tant que composant modifiable dans l’éditeur universel, en le liant à un modèle de bloc et éventuellement à un filtre.
- **Modèle** : spécifie les champs de création du bloc et la manière dont ces champs sont rendus en tant que code HTML Edge Delivery Services sémantique.
- **Filtre** : configure les règles de filtrage pour limiter les conteneurs auxquels le bloc peut être ajouté via l’éditeur universel. La plupart des blocs ne sont pas des conteneurs, mais leurs identifiants sont ajoutés aux filtres d’autres blocs de conteneurs.

Créez un fichier à l’adresse `/blocks/teaser/_teaser.json` avec la structure initiale suivante, dans l’ordre exact. Si les clés ne sont pas dans le bon ordre, elles risquent de ne pas se construire correctement.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### Modèle de bloc

Le modèle de bloc constitue une partie essentielle de la configuration du bloc, dans la mesure où il définit les éléments suivants :

1. L’expérience de création en définissant les champs disponibles pour modification.

   ![Champs de l’éditeur universel](./assets/5-new-block/fields-in-universal-editor.png)

2. Rendu des valeurs du champ en code HTML Edge Delivery Services.

Les modèles se voient attribuer un élément `id` correspondant à la [définition du bloc](#block-definition) et incluent un tableau `fields` pour spécifier les champs modifiables.

Chaque champ du tableau `fields` comporte un objet JSON qui inclut les propriétés requises suivantes :

| Propriété JSON | Description |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | Le [type de champ](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types), par exemple `text`, `reference` ou `aem-content`. |
| `name` | Le nom du champ, mappé à la propriété JCR où la valeur est stockée dans AEM. |
| `label` | Libellé affiché pour les personnes chargées de la création dans l’éditeur universel. |

Pour obtenir la liste complète des propriétés, y compris les propriétés facultatives, consultez la [documentation des champs de l’éditeur universel](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields).

#### Conception de bloc

![Bloc de teaser](./assets/5-new-block/block-design.png)

Le bloc de teaser comprend les éléments modifiables suivants :

1. **Image** : représente le contenu visuel du teaser.
2. **Contenu texte** : inclut le titre, le corps du texte et le bouton d’appel à l’action (CTA), et se trouve dans un rectangle blanc.
   - Les **titre** et le **corps de texte** peuvent être créés dans le même éditeur de texte enrichi.
   - Le bouton **CTA** peut être créé à partir d’un champ `text` pour le **libellé** et d’un champ `aem-content` pour le **lien**.

La conception du bloc de teaser est divisée en ces deux composants logiques (contenu d’image et de texte), ce qui garantit une expérience de création structurée et intuitive pour les utilisateurs et les utilisatrices.

### Champs de bloc

Définissez les champs requis pour le bloc : image, texte secondaire de l’image, texte, libellé CTA et lien CTA.

>[!BEGINTABS]

>[!TAB Manière correcte]

**Cet onglet illustre la manière correcte de modéliser le bloc de teaser.**

Le teaser se compose de deux zones logiques : image et texte. Pour simplifier le code nécessaire à l’affichage du code HTML Edge Delivery Services en tant qu’expérience web souhaitée, le modèle de bloc doit refléter cette structure.

- Regroupez les éléments **image** et **texte de remplacement d’image** à l’aide de la [réduction du champ](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).
- Regroupez les champs de contenu texte à l’aide des éléments [regroupement d’éléments](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) et [réduction du champ pour le CTA](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).

Si vous ne connaissez pas les notions de [réduction de champ](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse), [regroupement d’éléments](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) ou [inférence de type](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference), consultez la documentation associée avant de continuer, car elles sont essentielles pour créer un modèle de bloc bien structuré.

Dans l’exemple ci-dessous :

- L’[inférence de type](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) est utilisée pour créer automatiquement un élément HTML `<img>` à partir du champ `image`. La réduction du champ est utilisée avec les champs `image` et `imageAlt` pour créer un élément HTML `<img>`. L’attribut `src` est défini sur la valeur du champ `image`, tandis que l’attribut `alt` est défini sur la valeur du champ `imageAlt`.
- `textContent` est un nom de groupe utilisé pour classer les champs. Il doit être sémantique, mais peut désigner n’importe quel élément unique de ce bloc. Cela indique à l’éditeur universel de rendre tous les champs avec ce préfixe dans le même élément `<div>` dans la sortie HTML finale.
- La réduction du champ est également appliquée dans le groupe `textContent` pour l’appel à l’action (CTA). Le CTA est créé en tant que `<a>` via une [inférence de type](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference). Le champ `cta` est utilisé pour définir l’attribut `href` de l’élément `<a>` et le champ `ctaText` fournit le contenu textuel du lien dans les balises `<a ...>`.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

Ce modèle définit les entrées de création dans l’éditeur universel pour le bloc.

Le code HTML Edge Delivery Services obtenu pour ce bloc place l’image dans la première balise div et les champs `textContent` de groupe d’éléments dans la seconde balise div.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

Comme illustré [dans le chapitre suivant](./7a-block-css.md), cette structure HTML simplifie la mise en forme du bloc en tant qu’unité cohérente.

Pour comprendre les conséquences de l’absence d’utilisation de la réduction de champ et du regroupement d’éléments, consultez l’onglet **Manière incorrecte** ci-dessus.

>[!TAB Manière incorrecte]

**Cet onglet illustre une manière sous-optimale de modélisation du bloc de teaser, et n’est qu’une juxtaposition à la manière correcte.**

Il peut sembler tentant de définir chaque champ comme champ autonome dans le modèle de bloc sans utiliser la [réduction du champ](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) et le [regroupement d’éléments](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping). Cependant, cette omission complique la mise en forme du bloc en tant qu’unité cohérente.

Par exemple, le modèle de teaser peut être défini **sans** réduction du champ ni regroupement d’éléments comme suit :

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

Le code HTML Edge Delivery Services pour le bloc effectue le rendu de la valeur de chaque champ dans un élément `div` distinct, ce qui complique la compréhension du contenu, l’application du style et les ajustements de structure HTML pour obtenir la conception souhaitée.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

Chaque champ est isolé dans son propre `div`, ce qui complique le style de l’image et du contenu textuel en tant qu’unités cohérentes. Il est possible d’obtenir la conception souhaitée avec effort et créativité, mais l’utilisation du [regroupement des éléments](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) pour regrouper les champs de contenu de texte et de la [réduction du champ](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) pour ajouter des valeurs créées en tant qu’attributs d’élément est plus simple, plus facile et sémantiquement correcte.

Consultez l’onglet **Manière correcte** ci-dessus pour savoir comment mieux modéliser le bloc de teaser.

>[!ENDTABS]


### Définition du bloc

La définition de bloc enregistre le bloc dans l’éditeur universel. Voici une répartition des propriétés JSON utilisées dans la définition de bloc :

| Propriété JSON | Description |
|---------------|-------------|
| `definition.title` | Titre du bloc tel qu’il est affiché dans les blocs **Ajouter** de l’éditeur universel. |
| `definition.id` | Identifiant unique du bloc, utilisé pour contrôler son utilisation dans `filters`. |
| `definition.plugins.xwalk.page.resourceType` | Définit le type de ressource Sling pour le rendu du composant dans l’éditeur universel. Utilisez toujours un type de ressource `core/franklin/components/block/v#/block`. |
| `definition.plugins.xwalk.page.template.name` | Nom du bloc. Il doit être en minuscules et séparé par des tirets pour correspondre au nom de dossier du bloc. Cette valeur est également utilisée pour libeller l’instance du bloc dans l’éditeur universel. |
| `definition.plugins.xwalk.page.template.model` | Associe cette définition à sa définition `model`, qui contrôle les champs de création affichés pour le bloc dans l’éditeur universel. La valeur ici doit correspondre à une valeur `model.id`. |
| `definition.plugins.xwalk.page.template.classes` | Propriété facultative, dont la valeur est ajoutée à l’attribut `class` de l’élément HTML du bloc. Cela permet d’avoir des variantes d’un même bloc. La valeur `classes` peut être rendue modifiable en [ajoutant un champ de classes](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options) au [modèle](#block-model) du bloc. |


Voici un exemple JSON pour la définition de bloc :

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```json
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

Dans cet exemple :

- Le bloc est nommé « Teaser » et utilise le modèle `teaser` qui détermine les champs disponibles pour modification dans l’éditeur universel.
- Le bloc comprend le contenu par défaut du champ `textContent_text`, qui est une zone de texte enrichi pour le titre et le corps du texte, ainsi que des éléments `textContent_cta` et `textContent_ctaText` pour le lien et le libellé CTA (appel à l’action). Les noms des champs du modèle contenant le contenu initial correspondent aux noms des champs définis dans le [tableau de champs du modèle de contenu](#block-model).

Cette structure garantit que le bloc est configuré dans l’éditeur universel avec les champs, le modèle de contenu et le type de ressource appropriés pour le rendu.

### Filtres de bloc

Le tableau de `filters` du bloc définit, pour les [blocs de conteneur](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), les autres blocs qui peuvent être ajoutés au conteneur. Les filtres définissent une liste d’identifiants de bloc (`model.id`) qui peuvent être ajoutés au conteneur.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

Le composant Teaser n’est pas un [bloc de conteneur](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), ce qui signifie que vous ne pouvez pas y ajouter d’autres blocs. Par conséquent, son tableau `filters` reste vide. Au lieu de cela, ajoutez l’identifiant du teaser à la liste de filtres du bloc de section, de sorte que le teaser puisse être ajouté à une section.

![Filtres de bloc](./assets/5-new-block/filters.png)

Les blocs fournis par Adobe, tels que le bloc de section, stockent les filtres dans le dossier `models` du projet. Pour effectuer des réglages, recherchez le fichier JSON pour le bloc fourni par Adobe (par exemple, `/models/_section.json`) et ajoutez l’identifiant du teaser (`teaser`) à la liste des filtres. La configuration indique à l’éditeur universel que le composant Teaser peut être ajouté au bloc de conteneur de section.

[!BADGE /models/_section.json]{type=Neutral tooltip="Nom de fichier de l’exemple de code ci-dessous."}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

L’identifiant de définition de bloc de teaser `teaser` est ajouté au tableau `components`.

## Appliquer lint à vos fichiers JSON

Veillez à [appliquer lint fréquemment](./3-local-development-environment.md#linting) à vos modifications afin qu’elles soient claires et cohérentes. L’application fréquente de lint permet de détecter les problèmes tôt et réduit le temps de développement global. La commande `npm run lint:js` applique également lint aux fichiers JSON et détecte toute erreur de syntaxe.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## Créer le JSON de projet

Après la configuration des fichiers JSON du bloc (par exemple, `blocks/teaser/_teaser.json`, `models/_section.json`), ils sont automatiquement compilés dans les fichiers `component-models.json`, `component-definitions.json` et `component-filters.json` du projet. Cette compilation est gérée automatiquement par un hook de pré-validation [Husky](https://typicode.github.io/husky/) inclus dans le [modèle de projet AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk).

Les générations peuvent également être déclenchées manuellement ou par programmation à l’aide des scripts NPM de [génération JSON](./3-local-development-environment.md#build-json-fragments) du projet.

## Déployer le JSON de bloc

Pour rendre le bloc disponible dans l’éditeur universel, le projet doit être validé et placé dans la branche d’un référentiel GitHub, dans ce cas la branche `teaser`.

Le nom de branche exact qu’utilise l’éditeur universel peut être ajusté, par utilisateur ou utilisatrice, via l’URL de l’éditeur universel.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
# JSON files are compiled automatically and added to the commit via a husky precommit hook
$ git push origin teaser
```

Lorsque l’éditeur universel est ouvert avec le paramètre de requête `?ref=teaser`, le nouveau bloc `teaser` apparaît dans la palette du bloc. Notez que le bloc n’a pas de style : il effectue le rendu des champs du bloc en tant que code HTML sémantique, stylisé uniquement via le [CSS global](./4-website-branding.md#global-css).
