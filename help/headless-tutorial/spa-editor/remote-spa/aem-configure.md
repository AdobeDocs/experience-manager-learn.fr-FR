---
title: Configurer AEM pour l’éditeur de SPA et la SPA distante
description: Un projet AEM est requis pour la configuration et la prise en charge des exigences en matière de contenu afin de permettre à l’éditeur de SPA d’AEM de créer une SPA distante.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 100%

---

# Configurer AEM pour l’éditeur de SPA

Bien que la base de code SPA soit gérée en dehors d’AEM, un projet AEM est nécessaire pour configurer la prise en charge des exigences en matière de contenu. Ce chapitre décrit la création d’un projet AEM qui contient les configurations nécessaires :

+ Proxys des composants principaux de gestion de contenu web d’AEM
+ Proxy de page SPA distante AEM
+ Modèles de page SPA distante AEM
+ Pages AEM de SPA distante de référence
+ Sous-projet pour définir une SPA dans les mappages URL d’AEM
+ Dossiers de configuration OSGi

## Télécharger le projet de base à partir de GitHub

Téléchargez le projet `aem-guides-wknd-graphql` à partir de Github.com. Il contiendra certains fichiers de référence utilisés dans ce projet.

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## Créer un projet AEM

Créez un projet AEM dans lequel les configurations et le contenu de référence sont gérés. Ce projet sera généré dans le dossier `remote-spa-tutorial` du projet `aem-guides-wknd-graphql` cloné.

_Utilisez toujours la dernière version de l’[archétype AEM](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_La dernière commande renomme simplement le dossier de projet AEM afin qu’il soit clair qu’il s’agit du projet AEM, qui ne doit pas être confondu avec la SPA distante._

Alors que `frontendModule="react"` est spécifié, le projet `ui.frontend` n’est pas utilisé pour le cas d’utilisation de SPA distante. La SPA est développée et gérée en externe sur AEM et utilise uniquement l’API de contenu d’AEM. L’indicateur `frontendModule="react"` est requis pour que le projet inclue les dépendances Java™ AEM `spa-project` et configure les modèles de page de SPA distante.

L’archétype de projet AEM génère les éléments suivants qui servent à configurer AEM pour l’intégration à la SPA.

+ __Proxys des composants principaux de gestion de contenu web AEM__ sur `ui.apps/src/.../apps/wknd-app/components`.
+ __Proxy de page distante SPA AEM__ sur `ui.apps/src/.../apps/wknd-app/components/remotepage`.
+ __Modèles de page AEM__ sur `ui.content/src/.../conf/wknd-app/settings/wcm/templates`.
+ __Sous-projet pour définir les mappages de contenu__ sur `ui.content/src/...`.
+ __Pages d’AEM de SPA distante de référence__ sur `ui.content/src/.../content/wknd-app`.
+ __Dossiers de configuration OSGi__ sur `ui.config/src/.../apps/wknd-app/osgiconfig`.

Avec le projet AEM de base généré, quelques réglages assurent la compatibilité de l’éditeur de SPA avec les SPA distantes.

## Supprimer le projet ui.frontend

Comme la SPA est une SPA distante, supposons qu’elle est développée et gérée en dehors du projet AEM. Pour éviter les conflits, supprimez le projet `ui.frontend` du déploiement. Si le projet `ui.frontend` n’est pas supprimé, deux SPA, la SPA par défaut fournie dans le projet `ui.frontend` et la SPA distante, sont chargées simultanément dans l’éditeur de SPA d’AEM.

1. Ouvrez le projet AEM (`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`) dans votre IDE.
1. Ouvrez la racine `pom.xml`.
1. Commentez le `<module>ui.frontend</module` à partir de la liste `<modules>`.

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   Le fichier `pom.xml` doit se présenter comme suit :

   ![Suppression du module ui.frontend du pom du réacteur.](./assets/aem-project/uifrontend-reactor-pom.png)

1. Ouvrez le `ui.apps/pom.xml`.
1. Commentez le `<dependency>` sur `<artifactId>wknd-app.ui.frontend</artifactId>`.

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   Le fichier `ui.apps/pom.xml` doit se présenter comme suit :

   ![Suppression de la dépendance ui.frontend d’ui.apps.](./assets/aem-project/uifrontend-uiapps-pom.png)

Si le projet AEM a été créé avant ces modifications, supprimez manuellement la bibliothèque cliente `ui.frontend` générée à partir du projet `ui.apps` sur `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Mappage du contenu AEM

Pour qu’AEM charge la SPA distante dans l’éditeur de SPA, les mappages entre les itinéraires de SPA et les pages AEM utilisés pour l’ouverture et la création de contenu doivent être établis.

L’importance de cette configuration sera explorée ultérieurement.

Le mappage peut être effectué avec le [mappage Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) défini dans `/etc/map`.

1. Dans l’IDE, ouvrez le sous-projet `ui.content`.
1. Accédez à `src/main/content/jcr_root`.
1. Créez un dossier `etc`.
1. Dans `etc`, créez un dossier `map`.
1. Dans `map`, créez un dossier `http`.
1. Dans `http`, créez un fichier `.content.xml` avec les contenus :

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. Dans `http`, créez un dossier `localhost_any`.
1. Dans `localhost_any`, créez un fichier `.content.xml` avec les contenus :

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. Dans `localhost_any`, créez un dossier `wknd-app-routes-adventure`.
1. Dans `wknd-app-routes-adventure`, créez un fichier `.content.xml` avec les contenus :

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Ajoutez les nœuds de mappage à `ui.content/src/main/content/META-INF/vault/filter.xml` pour les inclure dans le package AEM.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

La structure de dossiers et les fichiers `.context.xml` doivent se présenter comme suit :

![Mappage Sling.](./assets/aem-project/sling-mapping.png)

Le fichier `filter.xml` doit se présenter comme suit :

![Mappage Sling.](./assets/aem-project/sling-mapping-filter.png)

Désormais, lorsque le projet AEM est déployé, ces configurations sont automatiquement incluses.

Les effets de mappage Sling AEM exécutés sur `http` et `localhost` ne prennent en charge que le développement local. Lors d’un déploiement sur AEM as a Cloud Service, des mappages Sling similaires doivent être ajoutés au `https` de la cible et au(x) domaine(s) AEM as a Cloud Service approprié(s). Pour plus d’informations, consultez la [documentation sur le mappage Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Stratégies de sécurité du partage de ressources entre origines multiples (CORS)

Configurez ensuite AEM pour protéger le contenu afin que seul cette SPA puisse accéder au contenu AEM. Configurez [Partage de ressources entre origines multiples (CORS) dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html?lang=fr).

1. Dans votre IDE, ouvrez le sous-projet Maven `ui.config`.
1. Accédez à `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`.
1. Créez un fichier nommé `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`.
1. Ajoutez les éléments suivants au fichier :

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

Le fichier `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` doit se présenter comme suit :

![Configuration CORS de l’éditeur de SPA.](./assets/aem-project/cors-configuration.png)

Les principaux éléments de configuration sont :

+ `alloworigin` indique les hôtes autorisés à récupérer du contenu d’AEM.
   + `localhost:3000` est ajouté à la prise en charge de la SPA s’exécutant localement.
   + `https://external-hosted-app` agit comme un espace réservé à remplacer par le domaine sur lequel la SPA distante est hébergée.
+ `allowedpaths` spécifie les chemins d’accès dans AEM couverts par cette configuration CORS. Le paramètre par défaut permet d’accéder à tout le contenu d’AEM, mais il ne peut être défini que sur les chemins d’accès spécifiques auxquels la SPA peut accéder, par exemple : `/content/wknd-app`.

## Définir la page AEM en tant que modèle de page SPA distante

L’archétype de projet AEM génère un projet prêt à l’intégration d’AEM avec une SPA distante, cependant un petit réglage important est nécessaire dans la structure de page d’AEM générée automatiquement. Le type de page d’AEM générée automatiquement doit être remplacé par __page SPA distante__, au lieu de __page SPA__.

1. Dans votre IDE, ouvrez le sous-projet `ui.content`.
1. Ouvrez vers `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`.
1. Mettez à jour ce fichier `.content.xml` avec :

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

Les modifications clés sont des mises à jour vers les nœuds `jcr:content` :

+ `cq:template` vers `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` vers `wknd-app/components/remotepage`

Le fichier `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` doit se présenter comme suit :

![Mises à jour de la page d’accueil .content.xml.](./assets/aem-project/home-content-xml.png)

Ces modifications permettent à cette page, qui agit comme la racine de la SPA dans AEM, de charger la SPA distante dans l’éditeur de SPA.

>[!NOTE]
>
>Si ce projet a été précédemment déployé sur AEM, veillez à supprimer la page AEM en tant que __Sites > Application WKND > us > en > Page d’accueil de l’application WKND__, étant donné que le projet `ui.content` est défini sur __fusion__ des nœuds plutôt que sur __mise à jour__.

Cette page peut également être supprimée et recréée en tant que page SPA distante directement dans AEM, mais étant donné qu’elle est créée automatiquement dans le projet `ui.content`, il est préférable de la mettre à jour dans la base de code.

## Déployer le projet AEM dans le SDK d’AEM

1. Assurez-vous que le service de création d’AEM s’exécute sur le port 4502.
1. Dans la ligne de commande, accédez à la racine du projet Maven d’AEM.
1. Utilisez Maven pour déployer le projet vers votre service de création du SDK d’AEM local.

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install - PautoInstallSinglePackage.](./assets/aem-project/mvn-install.png)

## Configurer la page racine d’AEM

Une fois le projet AEM déployé, une dernière étape est nécessaire pour préparer l’éditeur de SPA à charger la SPA distante. Dans AEM, marquez la page AEM correspondant à la racine de la SPA,`/content/wknd-app/us/en/home`, générée par l’archétype de projet AEM.

1. Connectez-vous au service de création AEM.
1. Accédez à __Sites > Application WKND > us > en__.
1. Sélectionnez __Page d’accueil de l’application WKND__ puis __Propriétés__.

   ![Page d’accueil de l’application WKND - Propriétés.](./assets/aem-content/edit-home-properties.png)

1. Accédez à l’onglet __SPA__.
1. Remplissez la __Configuration des SPA distantes__.
   + __URL de l’hôte SPA__ : `http://localhost:3000`.
      + URL de la racine de la SPA distante.

   ![Page d’accueil de l’application WKND - Configuration de la SPA distante.](./assets/aem-content/remote-spa-configuration.png)

1. Appuyez sur __Enregistrer et fermer__.

N’oubliez pas que nous avons remplacé le type de cette page par __Page SPA distante__, ce qui nous permet de voir l’onglet __SPA__ dans __Propriétés de la page__.

Cette configuration ne doit être définie que sur la page AEM correspondant à la racine de la SPA. Toutes les pages AEM situées sous cette page héritent de cette valeur.

## Félicitations.

Vous avez maintenant préparé les configurations AEM et vous les avez déployées sur votre instance de création AEM locale. Vous savez maintenant comment :

+ Supprimer la SPA générée par l’archétype de projet AEM en commentant les dépendances dans `ui.frontend`.
+ Ajouter des mappages Sling à AEM qui mappent les itinéraires SPA aux ressources dans AEM.
+ Configurer des stratégies de sécurité de partage de ressources entre origines multiples (CORS) permettant à la SPA distante d’utiliser du contenu provenant d’AEM.
+ Déployer un projet AEM sur votre service de création de SDK d’AEM local.
+ Marquer une page AEM en tant que racine SPA distante à l’aide de la propriété de page URL d’hôte de la SPA.

## Étapes suivantes

Une fois AEM configuré, nous pouvons nous concentrer sur [l’amorçage de la SPA distante](./spa-bootstrap.md) avec la prise en charge des domaines modifiables à l’aide de l’éditeur de SPA d’AEM.
