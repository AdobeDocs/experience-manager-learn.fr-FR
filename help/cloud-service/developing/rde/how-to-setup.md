---
title: Configuration de l’environnement de développement rapide
description: Découvrez comment configurer un environnement de développement rapide pour AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 4%

---


# Configuration de l’environnement de développement rapide

En savoir plus **configuration** Environnement de développement rapide (RDE) en AEM as a Cloud Service.

Cette vidéo montre :

- Ajout d’un RDE à votre programme à l’aide de Cloud Manager
- Flux de connexion RDE à l’aide d’Adobe IMS, similaire à tout autre environnement AEM as a Cloud Service
- Configuration de [Interface de ligne de commande extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) également appelé `aio CLI`
- Configuration et configuration de AEM RDE et Cloud Manager `aio CLI` plugin

>[!VIDEO](https://video.tv.adobe.com/v/3415490/?quality=12&learn=on)

## Prérequis

Les logiciels suivants doivent être installés localement :

- [Node.js](https://nodejs.org/en/) (LTS - Prise en charge à long terme)
- [npm 8+](https://docs.npmjs.com/)

## Configuration locale

Pour déployer le [Projet Sites WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) code et contenu sur l’éditeur de texte enrichi à partir de votre ordinateur local, procédez comme suit.

### Interface de ligne de commande extensible de Adobe I/O Runtime

Installez l’interface de ligne de commande extensible de Adobe I/O Runtime, également appelée `aio CLI` en exécutant la commande suivante à partir de la ligne de commande.

    &quot;shell
    $ npm install -g @adobe/aio-cli
    &quot;

### AEM plugins

Installez Cloud Manager et AEM modules externes RDE à l’aide de la fonction `aio cli`&#39;s `plugins:install` .

    &quot;shell
    $ plugins aio:install @adobe/aio-cli-plugin-cloudmanager
    
    $ plugins aio:install @adobe/aio-cli-plugin-aem-rde
    &quot;

Le module externe Cloud Manager permet aux développeurs d’interagir avec Cloud Manager à partir de la ligne de commande.

Le module externe RDE AEM permet aux développeurs de déployer du code et du contenu à partir de l’ordinateur local.

En outre, pour mettre à jour les modules externes, utilisez la variable `aio plugins:update` .

## Configuration des modules externes d’AEM

Les modules externes AEM doivent être configurés pour interagir avec votre RDE. Tout d’abord, à l’aide de l’interface utilisateur de Cloud Manager, copiez les valeurs de l’ID d’organisation, de programme et d’environnement.

1. ID d’organisation : Copiez la valeur de **Image de profil > Informations sur le compte (interne) > Fenêtre modale > ID d’organisation actuel**

   ![Identifiant d’organisation](./assets/Org-ID.png)

1. ID de programme : Copiez la valeur de **Aperçu du programme > Environnements > {NomProgramme}-rde > URI du navigateur > nombres entre `program/` et`/environment`**

1. ID d’environnement : Copiez la valeur de **Aperçu du programme > Environnements > {ProgramName}-rde > URI du navigateur > nombres après`environment/`**

   ![Identifiant de programme et d’environnement](./assets/Program-Environment-Id.png)

1. Ensuite, en utilisant la variable `aio cli`&#39;s `config:set` définissez ces valeurs en exécutant la commande suivante.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Vous pouvez vérifier les valeurs de configuration actuelles en exécutant la commande suivante.

    &quot;shell
    $ aio config:list
    &quot;

En outre, pour changer ou savoir à quelle organisation vous êtes actuellement connecté, vous pouvez utiliser la commande ci-dessous.

    &quot;shell
    $ aio où
    &quot;

## Vérification de l’accès RDE

Vérifiez l’installation et la configuration du module externe RDE d’AEM en exécutant la commande suivante.

    &quot;shell
    $ aio aem:rde:status
    &quot;

Les informations d’état RDE s’affichent comme l’état d’environnement, la liste des _votre projet AEM_ lots et configurations sur les services de création et de publication.

## Étape suivante

En savoir plus [utilisation](./how-to-use.md) un RDE pour déployer le code et le contenu de votre environnement de développement intégré (IDE) favori afin d’accélérer les cycles de développement.


## Ressources supplémentaires

[Activation de RDE dans une documentation de programme](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Configuration de [Interface de ligne de commande extensible de Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) également appelé `aio CLI`

[Utilisation et commandes de l’interface de ligne de commande AIO](https://github.com/adobe/aio-cli#usage)

[Module d’interface de ligne de commande Adobe I/O Runtime pour les interactions avec AEM environnements de développement rapide](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Module d’interface de ligne de commande de Cloud Manager AIO](https://github.com/adobe/aio-cli-plugin-cloudmanager)
