---
title: Configurer un environnement de développement local
description: Configurez un environnement de développement local pour les sites fournis avec Edge Delivery Services et modifiables avec l’éditeur universel.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '994'
ht-degree: 100%

---

# Configurer un environnement de développement local

Un environnement de développement local est essentiel pour développer rapidement les sites web diffusés par Edge Delivery Services. L’environnement utilise du code développé localement lors de l’approvisionnement du contenu à partir d’Edge Delivery Services, ce qui permet aux développeurs et développeuses d’afficher instantanément les modifications de code. Une telle configuration prend en charge le développement et les tests rapides et itératifs.

Les outils et processus de développement d’un projet de site web Edge Delivery Services sont conçus pour être familiers aux développeurs et développeuses web et fournir une expérience de développement rapide et efficace.

## Topologie de développement

Cette vidéo présente une vue d’ensemble de la topologie de développement d’un projet de site web Edge Delivery Services modifiable avec l’éditeur universel.

>[!VIDEO](https://video.tv.adobe.com/v/3443980/?learn=on&enablevpops&captions=fre_fr)

+++Voir les détails supplémentaires sur la topologie de développement

- **Référentiel GitHub** :
   - **Objectif** : héberge le code du site web (CSS et JavaScript).
   - **Structure** : la **branche principale** contient du code prêt pour la production, tandis que les autres branches contiennent du code de travail.
   - **Fonctionnalité** : le code de n’importe quelle branche peut s’exécuter dans les environnements de **production** ou de **prévisualisation** sans affecter le site web actif.

- **Service de création AEM** :
   - **Objectif** : sert de référentiel de contenu canonique où le contenu du site web est modifié et géré.
   - **Fonctionnalité** : le contenu est lu et écrit par l’**éditeur universel**. Le contenu modifié est publié dans **Edge Delivery Services** dans les environnements de **production** ou de **prévisualisation**.

- **Éditeur universel** :
   - **Objectif** : fournit une interface WYSIWYG pour la modification du contenu du site web.
   - **Fonctionnalité** : lit et écrit dans le **service de création AEM**. Peut être configuré pour utiliser le code de n’importe quelle branche du **référentiel GitHub** afin de tester et valider les modifications.

- **Edge Delivery Services** :
   - **Environnement de production** :
      - **Objectif** : diffuse le contenu et le code du site web en direct aux utilisateurs et utilisatrices finaux.
      - **Fonctionnalité** : diffuse le contenu publié à partir du **service de création AEM** à l’aide du code de la **branche principale** du **référentiel GitHub**.
   - **Environnement de prévisualisation** :
      - **Objectif** : fournit un environnement d’évaluation pour tester et prévisualiser le contenu et le code avant le déploiement.
      - **Fonctionnalité** : diffuse le contenu publié à partir du **service de création AEM** à l’aide du code de n’importe quelle branche du **référentiel GitHub**, ce qui permet d’effectuer des tests approfondis sans affecter le site web actif.

- **Environnement de développement local** :
   - **Objectif** : permet aux développeurs et développeuses d’écrire et de tester du code (CSS et JavaScript) localement.
   - **Structure** :
      - Clone local du **référentiel GitHub** pour le développement basé sur les branches.
      - L’**interface de ligne de commande AEM**, qui agit comme un serveur de développement, applique des modifications de code local à l’**environnement de prévisualisation** pour une expérience de rechargement à chaud.
   - **Workflow** : les développeurs et développeuses écrivent du code localement, valident les modifications dans une branche de travail, transmettent la branche vers GitHub, le valident dans l’**éditeur universel** (à l’aide de la branche spécifiée) et le fusionnent dans la **branche principale** lorsqu’il est prêt pour le déploiement en production.

+++

## Prérequis

Avant de commencer le développement, installez les éléments suivants sur votre ordinateur :

1. [Git](https://git-scm.com/)
1. [Node.js et npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (ou un éditeur de code similaire)

## Cloner le référentiel GitHub

Clonez le [référentiel GitHub créé dans le chapitre Nouveau projet de code](./1-new-code-project.md) qui contient le projet de code AEM Edge Delivery Services dans votre environnement de développement local.

![Clone de référentiel GitHub](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Un nouveau dossier `aem-wknd-eds-ue` est créé dans le répertoire `Code`, qui sert de racine au projet. Bien que le projet puisse être cloné vers n’importe quel emplacement de l’ordinateur, ce tutoriel utilise `~/Code` comme répertoire racine.

## Installer des dépendances de projet

Accédez au dossier du projet et installez les dépendances requises avec `npm install`. Bien que les projets Edge Delivery Services n’utilisent pas les systèmes de génération Node.js traditionnels tels que Webpack ou Vite, ils nécessitent toujours plusieurs dépendances pour le développement local.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## Installer l’interface de ligne de commande AEM

L’interface de ligne de commande AEM est un outil de ligne de commande Node.js conçu pour rationaliser le développement de sites web AEM basés sur Edge Delivery Services, fournissant un serveur de développement local pour le développement et le test rapides de votre site web.

Pour installer l’interface de ligne de commande AEM, exécutez ce qui suit :

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

L’interface de ligne de commande d’AEM peut également être installée globalement avec `npm install --global @adobe/aem-cli`.

## Démarrer le serveur de développement AEM local

La commande `aem up` démarre le serveur de développement local et ouvre automatiquement une fenêtre de navigateur sur l’URL du serveur. Ce serveur agit comme un proxy inverse de l’environnement Edge Delivery Services, en y diffusant du contenu tout en utilisant votre base de code locale pour le développement.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

L’interface de ligne de commande AEM ouvre le site web dans votre navigateur à l’adresse `http://localhost:3000/`. Les modifications apportées au projet sont automatiquement rechargées à chaud dans le navigateur web, tandis que les modifications du contenu [nécessitent une publication dans l’environnement de prévisualisation](./6-author-block.md) et une actualisation du navigateur web.

Si le site web s’ouvre avec une page 404, il est probable que les fichiers [fstab.yaml ou paths.json](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) mis à jour dans le [nouveau projet de code](./1-new-code-project.md) ne soient pas correctement configurés, ou que les modifications n’aient pas été validées dans la branche `main`.

## Créer des fragments JSON

Les projets Edge Delivery Services, créés à l’aide du [modèle AEM Boilerplate XWalk](https://github.com/adobe-rnd/aem-boilerplate-xwalk), reposent sur des configurations JSON qui activent la création de blocs dans l’éditeur universel.

- **Fragments JSON** : stockés avec leurs blocs associés et définissent les modèles, définitions et filtres de bloc.
   - **Fragments de modèle** : stockés à l’adresse `/blocks/example/_example.json`.
   - **Fragments de définition** : stockés à l’adresse `/blocks/example/_example.json`.
   - **Fragments de filtre** : stockés à l’adresse `/blocks/example/_example.json`.


Le [modèle de projet XWalk AEM Boilerplate](https://github.com/adobe-rnd/aem-boilerplate-xwalk) comprend un hook de pré-validation [Husky](https://typicode.github.io/husky/) qui détecte les modifications apportées aux fragments JSON et les compile dans les fichiers `component-*.json` appropriés lors de la `git commit`.

Bien que les scripts NPM suivants puissent être exécutés manuellement via `npm run` pour créer les fichiers JSON, cela est généralement inutile, car le hook de pré-validation Husky les gère automatiquement.

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| Script NPM | Description |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Crée tous les fichiers JSON à partir de fragments et les ajoute aux fichiers `component-*.json` appropriés. |
| `build:json:models` | Crée des fragments JSON de bloc et les compile dans `/component-models.json`. |
| `build:json:definitions` | Crée des fragments JSON de page et les compile dans `/component-definitions.json`. |
| `build:json:filters` | Crée des fragments JSON de page et les compile dans `/component-filters.json`. |

>[!TIP]
>
> Exécutez `npm run build:json` après toute modification apportée aux fichiers de fragment pour régénérer les fichiers JSON principaux.

## Linting

Le linting assure la qualité et la cohérence du code, ce qui est nécessaire pour les projets Edge Delivery Services avant de fusionner les modifications dans la branche `main`.

Les scripts NPM peuvent être exécutés via `npm run`, par exemple :

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| Script NPM | Description |
|------------------|--------------------------------------------------|
| `lint:js` | Exécute les vérifications de linting JavaScript. |
| `lint:css` | Exécute des vérifications de linting CSS. |
| `lint` | Exécute les vérifications de linting JavaScript et CSS. |

### Correction automatique des problèmes de linting

Vous pouvez résoudre automatiquement les problèmes de linting en ajoutant les `scripts` suivants au `package.json` du projet. Ils peuvent être exécutés via `npm run` :

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Ces scripts ne sont pas préconfigurés avec le modèle AEM Boilerplate XWalk, mais peuvent être ajoutés au fichier `package.json` :

>[!BEGINTABS]

>[!TAB Scripts supplémentaires]

| Script NPM | Commande | Description |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | Cela corrige automatique des problèmes de linting JavaScript. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | Cela corrige automatique des problèmes de linting CSS. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Exécute les scripts de correctif JS et CSS pour un nettoyage rapide. |

>[!TAB Exemple de package.json]

Les entrées de script suivantes peuvent être ajoutées au tableau de `scripts` `package.json`.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
