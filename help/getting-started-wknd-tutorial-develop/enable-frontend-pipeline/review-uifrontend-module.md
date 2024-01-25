---
title: Examinez le module ui.frontend du projet full-stack.
description: Examinez le cycle de vie du développement, du déploiement et de la diffusion front-end d’un projet AEM Sites full-stack basé sur Maven.
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 390
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 100%

---

# Examinez le module « ui.frontend » du projet AEM full-stack. {#aem-full-stack-ui-frontent}

Dans ce chapitre, nous passons en revue le développement, le déploiement et la diffusion des artefacts front-end d’un projet AEM full-stack, en nous concentrant sur le module « ui.frontend » du __projet Sites WKND__.


## Objectifs {#objective}

* Découvrir la version et le flux de déploiement des artefacts front-end dans un projet AEM full-stack.
* Examiner les configurations du [webpack](https://webpack.js.org/) du module du projet full-stack AEM `ui.frontend`.
* Processus de génération de la bibliothèque cliente AEM (également appelée clientlibs)

## Flux de déploiement front-end pour les projets AEM full-stack et de création rapide de site

>[!IMPORTANT]
>
>Cette vidéo explique et présente le flux front-end pour les projets **full-stack et de création rapide de site** afin de souligner la différence subtile dans le modèle de version, de déploiement et de diffusion des ressources front-end.

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## Prérequis {#prerequisites}


* Cloner le [projet AEM Sites WKND](https://github.com/adobe/aem-guides-wknd)
* Avoir créé et déployé le projet AEM Sites WKND cloné pour AEM as a Cloud Service.

Consultez le [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) du projet WKND Site d’AEM pour plus d’informations.

## Flux d’artefact front-end du projet full-stack AEM {#flow-of-frontend-artifacts}

Vous trouverez ci-dessous une représentation de haut niveau du flux de __développement, déploiement et diffusion__ des artefacts front-end dans un projet AEM full-stack.

![Développement, déploiement et diffusion d’artefacts front-end.](assets/Dev-Deploy-Delivery-AEM-Project.png)


Au cours de la phase de développement, les modifications front-end telles que le style et le changement d’image sont effectuées en mettant à jour les fichiers CSS et JS du dossier `ui.frontend/src/main/webpack`. Ensuite, au moment de la création, le module-bundler [webpack](https://webpack.js.org/) et le module externe Maven transforment ces fichiers en bibliothèques clientes AEM optimisées dans le module `ui.apps`.

Les modifications front-end sont déployées dans l’environnement AEM as a Cloud Service lors de l’exécution du pipeline [__full-stack__ dans Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr).

Les ressources front-end sont diffusées aux navigateurs web via des chemins URI commençant par `/etc.clientlibs/`, et sont généralement mises en cache sur AEM Dispatcher et le réseau CDN.


>[!NOTE]
>
> De même, dans le __parcours de création rapide de site AEM__, les [modifications front-end](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=fr) sont déployées dans l’environnement AEM as a Cloud Service en exécutant le pipeline __front-end__. Consultez [Configuration de votre pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=fr).

### Examiner les configurations webpack dans le projet Sites WKND {#development-frontend-webpack-clientlib}

* Il existe trois fichiers de configuration __webpack__ utilisés pour regrouper les ressources front-end du projet Sites WKND.

   1. `webpack.common` contient la configuration __commune__ pour indiquer le regroupement et l’optimisation des ressources WKND. La propriété __output__ indique où émettre les fichiers consolidés (également appelés lots JavaScript, mais à ne pas confondre avec les lots OSGi AEM) qu’elle crée. Le nom par défaut est défini sur `clientlib-site/js/[name].bundle.js`.

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` contient la configuration de __développement__ utilisée pour le webpack-dev-server et pointe vers le modèle HTML à utiliser. Il comprend également une configuration proxy utilisée pour une instance AEM s’exécutant sur `localhost:4502`.

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` contient la configuration de __production__ et utilise les modules externes pour transformer les fichiers de développement en lots optimisés.

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* Les ressources regroupées sont déplacées vers le module `ui.apps` en utilisant le module externe [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) à l’aide de la configuration gérée dans le fichier `clientlib.config.js`.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* Le plug-in __frontend-maven-plugin__ de `ui.frontend/pom.xml` orchestre le regroupement webpack et la génération de la bibliothèque cliente pendant la création du projet AEM.

`$ mvn clean install -PautoInstallSinglePackage`

### Déploiement sur AEM as a Cloud Service {#deployment-frontend-aemaacs}

Le [__pipeline__ full-stack](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=fr#full-stack-pipeline) déploie ces modifications sur un environnement AEM as a Cloud Service.


### Diffusion depuis AEM as a Cloud Service {#delivery-frontend-aemaacs}

Les ressources front-end déployées via le pipeline full-stack sont diffusées à partir d’AEM Sites vers les navigateurs web sous la forme de fichiers `/etc.clientlibs`. Vous pouvez le vérifier en consultant le [site WKND hébergé publiquement](https://wknd.site/content/wknd/us/en.html) et en affichant la source de la page web.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Félicitations. {#congratulations}

Félicitations, vous avez examiné le module ui.frontend du projet full-stack.

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Mettre à jour le projet pour utiliser le pipeline front-end](update-project.md), vous allez mettre à jour le projet AEM Sites WKND pour l’activer pour le contrat de pipeline front-end.
