---
title: Mettre à jour un projet AEM full-stack pour utiliser le pipeline front-end
description: Découvrez comment mettre à jour un projet AEM full-stack afin de l’activer pour le pipeline front-end, de sorte qu’il ne crée et ne déploie que les artefacts front-end.
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 343
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 100%

---

# Mettre à jour un projet AEM full-stack pour utiliser le pipeline front-end {#update-project-enable-frontend-pipeline}

Dans ce chapitre, nous apportons des modifications de configuration au __projet WKND Sites__ pour utiliser le pipeline front-end afin de déployer JavaScript et CSS, plutôt que d’avoir à exécuter un pipeline full-stack. Cela découple le cycle de vie du développement et du déploiement des artefacts front-end et back-end, ce qui permet d’accélérer le processus de développement itératif.

## Objectifs {#objectives}

* Mettre à jour le projet full-stack pour utiliser le pipeline front-end

## Présentation des modifications de configuration dans le projet AEM full-stack

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que vous avez examiné le [module « ui.frontend »](./review-uifrontend-module.md).


## Modifications apportées au projet AEM full-stack

Il existe trois modifications de configuration liées au projet et un changement de style à déployer pour une exécution de test, donc au total quatre modifications spécifiques à effectuer dans le projet WKND afin de l’activer pour le contrat de pipeline front-end.

1. Supprimez le module `ui.frontend` du cycle de création full-stack

   * Dans la racine `pom.xml` du projet WKND Sites, laissez un commentaire sur l’entrée du sous-module `<module>ui.frontend</module>`.

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * Laissez également un commentaire sur les dépendances associées dans `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. Préparez le module `ui.frontend` pour le contrat de pipeline front-end en ajoutant deux nouveaux fichiers de configuration webpack.

   * Copiez le `webpack.common.js` existant en tant que `webpack.theme.common.js`, modifiez la propriété `output`, `MiniCssExtractPlugin`, et les paramètres de configuration du plug-in `CopyWebpackPlugin` comme suit :

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Copiez le `webpack.prod.js` existant en tant que `webpack.theme.prod.js` et modifiez l’emplacement de la variable `common` dans le fichier ci-dessus comme suit :

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >Les deux modifications de la configuration « webpack » ci-dessus doivent comporter des noms de fichier et de dossier de sortie différents afin de pouvoir différencier facilement les artefacts clientlib (full-stack) des artefacts front-end générés par les thèmes.
   >
   >Comme vous l’avez deviné, les changements ci-dessus peuvent être ignorés pour utiliser les configurations webpack existantes, mais les modifications ci-dessous sont requises.
   >
   >C’est à vous de décider comment vous voulez les nommer ou les organiser.


   * Dans le fichier `package.json`, assurez-vous que la valeur de la propriété `name` est identique au nom du site du nœud `/conf`. Assurez vous également qu’un script `build` expliquant comment créer les fichiers front-end à partir de ce module se trouve sous la propriété `scripts`.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. Préparez le module `ui.content` pour le pipeline front-end en ajoutant deux configurations Sling.

   * Créez un fichier sous `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - cela inclut tous les fichiers front-end que le module `ui.frontend` génère sous le dossier `dist` à l’aide du processus de création webpack.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    Consultez la [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) complète dans le __projet AEM WKND Sites__.


   * Passez ensuite à `com.adobe.aem.wcm.site.manager.config.SiteConfig`, où la valeur `themePackageName` est identique à la valeur des propriétés `package.json` et `name`, et où `siteTemplatePath` pointe vers le chemin d’accès `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0`.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    Voir la section [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) complète dans le __projet AEM WKND Sites__.

1. Dans le cas d’une modification de thème ou de styles à déployer via le pipeline front-end pour une exécution de test, nous modifions la couleur `text-color` en rouge Adobe (ou vous pouvez choisir le vôtre) en mettant à jour `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Enfin, envoyez ces modifications au référentiel git d’Adobe de votre programme.


>[!AVAILABILITY]
>
> Ces modifications sont disponibles sur GitHub dans le [__pipeline front-end__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) de la branche du __projet AEM WKND Sites__.


## Attention - Bouton _Activer le pipeline front-end_

L’option [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=fr) du [sélecteur de rail](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html?lang=fr) affiche le bouton **Activer le pipeline front-end** lors de la sélection de la racine ou de la page de votre site. Cliquer sur le bouton **Activer le pipeline front-end** remplacera les **configurations Sling** ci-dessus. Assurez-vous de **ne pas cliquer** sur ce bouton après le déploiement des modifications ci-dessus via l’exécution du pipeline Cloud Manager.

![Bouton Activer le pipeline front-end](assets/enable-front-end-Pipeline-button.png)

Si vous cliquez dessus par erreur, vous devrez réexécuter les pipelines pour vous assurer que le contrat et les modifications du pipeline front-end sont restaurés.

## Félicitations ! {#congratulations}

Félicitations, vous avez mis à jour le projet WKND Sites afin de l’activer pour le contrat de pipeline front-end.

## Étapes suivantes {#next-steps}

Dans le chapitre suivant, [Déployer à l’aide du pipeline front-end](create-frontend-pipeline.md), vous allez créer et exécuter un pipeline front-end, et vous découvrirez comment __s’éloigner__ de la diffusion des ressources front-end « /etc.clientlibs ».
