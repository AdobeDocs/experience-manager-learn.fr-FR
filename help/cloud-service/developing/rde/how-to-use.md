---
title: Comment utiliser l’environnement de développement rapide
description: Découvrez comment utiliser l’environnement de développement rapide pour déployer du code et du contenu à partir de votre ordinateur local.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 841
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 100%

---

# Comment utiliser l’environnement de développement rapide

Découvrez l’**utilisation** de l’environnement de développement rapide (RDE) dans AEM as a Cloud Service. Déployez du code et du contenu pour accélérer les cycles de développement de votre code quasi-final vers le RDE, à partir de votre environnement de développement intégré (IDE) préféré.

Utilisez le [projet de sites WKND AEM](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) pour apprendre à déployer divers artefacts AEM sur le RDE en exécutant la commande `install` du RDE d’AEM à partir de votre IDE favori.

- Déploiement du code AEM et du package de contenu (tous, ui.apps)
- Déploiement du lot OSGi et du fichier de configuration
- Apache et Dispatcher configurent le déploiement en tant que fichier zip.
- Déploiement de fichiers individuels tels que HTL, `.content.xml` (boîte de dialogue XML)
- Examinez d’autres commandes du RDE, comme `status, reset and delete`.

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Prérequis

Clonez le projet [Sites WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) et ouvrez-le dans votre IDE favori pour déployer les artefacts AEM sur le RDE.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Ensuite, créez-le et déployez-le sur le SDK AEM local en exécutant la commande Maven suivante :

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Déploiement d’artefacts AEM à l’aide du plug-in AEM-RDE

À l’aide de la commande `aem:rde:install`, nous allons déployer divers artefacts AEM.

### Déployer les packages `all` et `dispatcher`.

Un point de départ courant consiste à déployer d’abord les packages `all` et `dispatcher` en exécutant les commandes suivantes :

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Une fois les déploiements réussis, vérifiez le site WKND sur les services de création et de publication. Vous devriez être en mesure d’ajouter et de modifier le contenu sur les pages du site WKND et de le publier.

### Améliorer et déployer un composant

Ajoutons le `Hello World Component` et déployons-le sur le RDE.

1. Ouvrez le fichier de boîte de dialogue XML (`.content.xml`) à partir du dossier `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/`.
1. Ajoutez le champ de texte `Description` après le champ de boîte de dialogue `Text` existant.

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Ouvrez le fichier `helloworld.html` à partir du dossier `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld`.
1. Effectuez le rendu de la propriété `Description` d’après l’élément `<div>` existant de la propriété `Text`.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Vérifiez les modifications sur le SDK AEM local en exécutant la création Maven ou en synchronisant les différents fichiers.

1. Déployez les modifications apportées au RDE via le package `ui.apps` ou en déployant les fichiers de boîte de dialogue et HTL.

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Vérifiez les modifications sur le RDE en ajoutant ou en modifiant le `Hello World Component` sur une page du site WKND.

### Examiner les options de commande `install`

Dans l’exemple de commande de déploiement de fichiers ci-dessus, les indicateurs `-t` et `-p` servent à indiquer respectivement le type et la destination du chemin JCR. Examinons les options de commande `install` en exécutant la commande suivante :

```shell
$ aio aem:rde:install --help
```

Les indicateurs sont explicites, l’indicateur `-s` est utile pour cibler le déploiement uniquement sur les services de création ou de publication. Utilisez l’indicateur `-t` lors du déploiement des fichiers **content-file ou content-xml** avec l’indicateur `-p` pour spécifier le chemin d’accès JCR de destination dans l’environnement RDE AEM.

### Déployer le lot OSGi

Pour savoir comment déployer le lot OSGi, améliorons la classe Java™ `HelloWorldModel` et déployons-la sur le RDE.

1. Ouvrez le fichier `HelloWorldModel.java` à partir du dossier `core/src/main/java/com/adobe/aem/guides/wknd/core/models`.
1. Mettez à jour la méthode `init()` comme ci-dessous :

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Vérifiez les modifications apportées au SDK AEM local en déployant le lot `core` via la commande Maven.
1. Déployez les modifications apportées au RDE en exécutant la commande suivante :

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Vérifiez les modifications sur le RDE en ajoutant ou en modifiant le `Hello World Component` sur une page du site WKND.

### Déployer la configuration OSGi

Vous pouvez déployer les différents fichiers de configuration ou le package de configuration complet, par exemple :

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>Pour installer une configuration OSGi uniquement sur une instance de création ou de publication, utilisez l’indicateur `-s`.


### Déployer la configuration Apache ou Dispatcher

Les fichiers de configuration Apache ou Dispatcher **ne peuvent pas être déployés individuellement**, mais toute la structure de dossiers de Dispatcher doit être déployée sous la forme d’un fichier ZIP.

1. Apportez la modification souhaitée dans le fichier de configuration du module `dispatcher`. À des fins de démonstration, mettez à jour le `dispatcher/src/conf.d/available_vhosts/wknd.vhost` pour mettre en cache les fichiers `html` uniquement pendant 60 secondes.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. Vérifiez les modifications localement, et consultez [Exécution locale de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=fr#ex%C3%A9cution-locale-de-dispatcher) pour plus d’informations.
1. Déployez les modifications apportées au RDE en exécutant la commande suivante :

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Vérifier les modifications apportées au RDE

## Commandes supplémentaires du plug-in du RDE AEM

Examinons les commandes supplémentaires du plug-in du RDE AEM pour gérer et interagir avec le RDE depuis votre ordinateur local.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

À l’aide des commandes ci-dessus, votre RDE peut être géré à partir de votre IDE favori pour accélérer le cycle de vie du développement/déploiement.

## Étape suivante

En savoir plus sur les [cycle de vie de développement/déploiement à l’aide de RDE](./development-life-cycle.md) pour fournir des fonctionnalités rapidement.


## Ressources supplémentaires

[Documentation relatives aux commandes du RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html?lang=fr#rde-cli-commands)

[Plug-in d’interface de ligne de commande Adobe I/O Runtime pour les interactions avec les environnements de développement rapide AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configuration du projet AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html?lang=fr)
