---
title: Utilisation de l’environnement de développement rapide
description: Découvrez comment utiliser l’environnement de développement rapide pour déployer du code et du contenu à partir de votre ordinateur local.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 1%

---


# Utilisation de l’environnement de développement rapide

En savoir plus **utilisation** Environnement de développement rapide (RDE) en AEM as a Cloud Service. Déployez du code et du contenu pour accélérer les cycles de développement de votre code quasi-final vers l’éditeur de texte enrichi (RDE), à partir de votre environnement de développement intégré (IDE) préféré.

Utilisation [AEM projet WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) vous apprenez à déployer divers artefacts AEM sur l’éditeur de texte enrichi en exécutant AEM-RDE `install` à partir de votre IDE favori.

- Déploiement du code AEM et du package de contenu (tous, ui.apps)
- Déploiement du lot OSGi et du fichier de configuration
- Apache et Dispatcher configurent le déploiement en tant que fichier zip.
- Fichiers individuels tels que HTL, `.content.xml` Déploiement (dialog XML)
- Examinez d’autres commandes RDE comme `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## Prérequis

Cloner le [Sites WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) et ouvrez-le dans votre IDE favori pour déployer les artefacts AEM sur l’éditeur de texte enrichi.

    &quot;shell
    Cloner $ git git@github.com:adobe/aem-guides-wknd.git
    &quot;

Ensuite, créez-le et déployez-le sur le AEM-SDK local en exécutant la commande maven suivante.

    &quot;
    $ cd aem-guides-wknd/
    $ mvn clean install -PautoInstallSinglePackage
    &quot;

## Déploiement d’artefacts AEM à l’aide du module externe AEM-RDE

En utilisant la variable `aem:rde:install` , déployons divers artefacts AEM.

### Déployer `all` et `dispatcher` packages

Un point de départ commun consiste à déployer d’abord la variable `all` et `dispatcher` en exécutant les commandes suivantes.

    &quot;shell
    # Installation du package &quot;all&quot;
    $ aio aem:rde:installez all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip.
    
    # Installation du fichier zip &#39;dispatcher&#39;
    $ aio aem:rde:installez dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip.
    &quot;

Une fois les déploiements réussis, vérifiez le site WKND sur les services de création et de publication. Vous devriez être en mesure d’ajouter et de modifier le contenu sur les pages du site WKND et de le publier.

### Amélioration et déploiement d’un composant

Ajoutons le `Hello World Component` et déployez-le sur l’éditeur de texte enrichi.

1. Ouvrez la boîte de dialogue XML (`.content.xml`) à partir de `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` folder
1. Ajoutez la variable `Description` champ de texte après le champ existant `Text` champ de boîte de dialogue

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Ouvrez le `helloworld.html` fichier à partir de `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` folder
1. Rendre le `Description` après la propriété existante `<div>` de l’élément `Text` .

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Vérifiez les modifications sur le AEM-SDK local en exécutant la création maven ou en synchronisant les fichiers individuels.

1. Déployez les modifications apportées au RDE via `ui.apps` ou en déployant les fichiers Dialog et HTL individuels.

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

1. Vérifier les modifications sur l’éditeur de texte enrichi en ajoutant ou en modifiant le `Hello World Component` sur une page du site WKND.

### Consultez la section `install` options de commande

Dans l’exemple de commande de déploiement de fichiers individuel ci-dessus, la commande `-t` et `-p` Les indicateurs sont utilisés pour indiquer respectivement le type et la destination du chemin JCR. Examinons les `install` en exécutant la commande suivante.

    &quot;shell
    $ aio aem:rde:install —help
    &quot;

Les drapeaux sont explicites, `-s` L’indicateur est utile pour cibler le déploiement uniquement sur les services de création ou de publication. Utilisez la variable `-t` Indicateur lors du déploiement de la variable **content-file ou content-xml** avec les fichiers `-p` Indicateur pour spécifier le chemin d’accès JCR de destination dans l’environnement RDE AEM.

### Déploiement du lot OSGi

Pour savoir comment déployer le bundle OSGi, améliorons la `HelloWorldModel` Classe Java™ et déployez-la sur le RDE.

1. Ouvrez le `HelloWorldModel.java` fichier à partir de `core/src/main/java/com/adobe/aem/guides/wknd/core/models` folder
1. Mettez à jour le `init()` comme ci-dessous :

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Vérifiez les modifications apportées au AEM-SDK local en déployant le `core` regroupement via la commande maven
1. Déployez les modifications apportées au RDE en exécutant la commande suivante :

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Vérifier les modifications sur l’éditeur de texte enrichi en ajoutant ou en modifiant le `Hello World Component` sur une page du site WKND.

### Déploiement de la configuration OSGi

Vous pouvez déployer les fichiers de configuration individuels ou terminer le package de configuration, par exemple :

    &quot;shell
    # Déploiement d’un fichier de configuration individuel
    $ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json
    
    # Ou déployer le package de configuration complet
    $ cd ui.config
    Package de nettoyage mvn $
    $ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
    &quot;

>[!TIP]
>
>Pour installer une configuration OSGi uniquement sur une instance d’auteur ou de publication, utilisez la méthode `-s` Indicateur.


### Déploiement de la configuration Apache ou Dispatcher

Les fichiers de configuration Apache ou Dispatcher **ne peuvent pas être déployés individuellement**, mais toute la structure de dossiers de Dispatcher doit être déployée sous la forme d’un fichier ZIP.

1. Apportez la modification souhaitée dans le fichier de configuration de la fonction `dispatcher` à des fins de démonstration, mettez à jour le module `dispatcher/src/conf.d/available_vhosts/wknd.vhost` pour mettre en cache la variable `html` uniquement pendant 60 secondes.

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

1. Vérifiez les modifications localement, voir [Exécution locale de Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) pour plus d’informations.
1. Déployez les modifications apportées au RDE en exécutant la commande suivante :

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Vérifier les modifications sur l’éditeur de texte enrichi

## Commandes AEM du module externe RDE supplémentaires

Examinons les commandes supplémentaires AEM du module externe RDE pour gérer et interagir avec le RDE depuis votre ordinateur local.

    &quot;shell
    $ aio aem:rde —help
    Interaction avec les environnements RapidDev.
    
    UTILISATION
    $ aio aem rde COMMANDE
    
    COMMANDES
    aem rde Supprimez les lots et les configurations de la règle actuelle.
    aem rde history Obtenez une liste des mises à jour apportées au code actuel.
    aem rde install les lots Install/update, les configurations et content-packages.
    réinitialisation de la règle d’aem Réinitialiser le RDE
    aem rde restart Redémarrez l’auteur et la publication d’un RDE.
    aem rde status Obtenez une liste des lots et des configurations déployés dans le code actuel.
    &quot;

À l’aide des commandes ci-dessus, votre RDE peut être géré à partir de votre IDE favori pour accélérer le cycle de vie du développement/déploiement.

## Étape suivante

En savoir plus sur les [cycle de vie de développement/déploiement à l’aide de RDE](./development-life-cycle.md) pour offrir des fonctionnalités rapidement.


## Ressources supplémentaires

[Documentation des commandes RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Module d’interface de ligne de commande Adobe I/O Runtime pour les interactions avec AEM environnements de développement rapide](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configuration AEM projet](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html?lang=fr)
