---
title: Configurer l’environnement de développement rapide
description: Découvrez comment configurer un environnement de développement rapide pour AEM as a Cloud Service.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '666'
ht-degree: 100%

---

# Configurer l’environnement de développement rapide

Découvrez **comment configurer** un environnement de développement rapide (RDE) dans AEM as a Cloud Service.

Cette vidéo montre les éléments suivants :

- Ajout d’un RDE à votre programme à l’aide de Cloud Manager.
- Flux de connexion RDE à l’aide d’Adobe IMS, similaire à tout autre environnement AEM as a Cloud Service.
- Configuration de l’[interface de ligne de commande extensible Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), également appelée la `aio CLI`.
- Configuration et paramétrage du plug-in `aio CLI` RDE AEM et Cloud Manager à l’aide du mode non interactif. Pour le mode interactif, voir les [instructions de configuration](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Prérequis

Les logiciels suivants doivent être installés localement :

- [Node.js](https://nodejs.org/fr/) (LTS - Prise en charge à long terme)
- [npm 8 et ultérieure.](https://docs.npmjs.com/)

## Configuration locale

Pour déployer le code et le contenu du [projet de site WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) sur le RDE à partir de votre ordinateur local, procédez comme suit :

### Interface de ligne de commande extensible Adobe I/O Runtime

Installez l’interface de ligne de commande extensible Adobe I/O Runtime, également appelée la `aio CLI`, en exécutant la commande suivante à partir de la ligne de commande.

```shell
$ npm install -g @adobe/aio-cli
```

### Installation et configuration des plug-ins de ligne de commande aio

L’interface de ligne de commande aio doit disposer de plug-ins installés et configurés avec l’ID d’environnement de l’organisation, du programme et du RDE pour interagir avec votre RDE. La configuration peut être effectuée via l’interface de ligne de commande aio à l’aide du mode interactif plus simple ou du mode non interactif.

>[!BEGINTABS]

>[!TAB Mode interactif]

Installez et configurez les plug-ins RDE AEM à l’aide de la commande `plugins:install` de la `aio cli`.

1. Installez le plug-in RDE AEM de l’interface de ligne de commande aio à l’aide de la commande `plugins:install` de la `aio cli`.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   Le plug-in RDE AEM permet aux développeurs et aux développeuses de déployer du code et du contenu à partir de l’ordinateur local.

2. Connectez-vous à l’interface de ligne de commande Extensible d’Adobe I/O Runtime en exécutant la commande suivante pour obtenir le jeton d’accès. Veillez à vous connecter à la même organisation Adobe que Cloud Manager.

   ```shell
   $ aio login
   ```

3. Exécutez la commande suivante pour configurer le RDE à l’aide du mode interactif.

   ```shell
   $ aio aem:rde:setup
   ```

4. L’interface en ligne de commande vous invite à saisir l’ID d’organisation, l’ID de programme et l’ID d’environnement.

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - Sélectionnez __Non__ si vous ne travaillez qu’avec un seul RDE et souhaitez stocker globalement votre configuration RDE sur votre ordinateur local.

   - Sélectionnez __Oui__ si vous utilisez plusieurs RDE ou souhaitez stocker votre configuration RDE localement, dans le fichier `.aio` du dossier actuel, pour chaque projet.

5. Sélectionnez les ID d’organisation, de programme et d’environnement RDE dans la liste des options disponibles.

6. Vérifiez que l’organisation, le programme et l’environnement appropriés sont configurés en exécutant la commande ci-après.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB Mode non interactif]

Installez les plug-ins RDE pour Cloud Manager et AEM à l’aide de la commande `plugins:install` de la `aio cli`.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Le plug-in Cloud Manager permet aux développeurs et aux développeuses d’interagir avec Cloud Manager à partir de la ligne de commande.

Le plug-in RDE AEM permet aux développeurs et aux développeuses de déployer du code et du contenu à partir de l’ordinateur local.

Les plug-ins de ligne de commande aio doivent être configurés pour interagir avec le RDE.

1. Tout d’abord, à l’aide de l’interface d’utilisation de Cloud Manager, copiez les valeurs des ID d’organisation, de programme et d’environnement.

   - ID d’organisation : copiez la valeur à partir de **Photo de profil > Informations sur le compte (interne) > Fenêtre modale > ID d’organisation actuel**.

   ![Identifiant d’organisation.](./assets/Org-ID.png)

   - ID de programme : copiez la valeur à partir de **Vue d’ensemble du programme > Environnements > {ProgramName}-rde > URI du navigateur > nombres entre `program/` et`/environment`**.

   ![ID de programme et d’environnement.](./assets/Program-Environment-Id.png)

   - ID d’environnement : copiez la valeur à partir de **Vue d’ensemble du programme > Environnements > {ProgramName}-rde > URI du navigateur > nombres après`environment/`**.

   ![ID de programme et d’environnement.](./assets/Program-Environment-Id.png)

1. Ensuite, en utilisant le jeu de commandes `config:set` de la `aio cli`, définissez ces valeurs en exécutant la commande ci-après.

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. Vérifiez les valeurs de configuration actuelles en exécutant la commande ci-après.

   ```shell
   $ aio config:list
   ```

1. Changez ou vérifiez l’organisation à laquelle votre connexion est actuellement établie :

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## Vérifier l’accès au RDE

Vérifiez l’installation et la configuration du plug-in du RDE AEM en exécutant la commande suivante.

```shell
$ aio aem:rde:status
```

Les informations sur le statut du RDE s’affichent, comme le statut de l’environnement et la liste des lots et des configurations de _votre projet AEM_ sur les services de création et de publication.

## Étape suivante

Découvrez [comment utiliser](./how-to-use.md) un RDE pour déployer le code et le contenu de votre environnement de développement intégré (IDE) favori afin d’accélérer les cycles de développement.


## Ressources supplémentaires

[Documentation sur l’activation du RDE dans un programme](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html?lang=fr#enabling-rde-in-a-program)

Configurer l’[interface de ligne de commande extensible Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), également appelée la `aio CLI`

[Utilisation et commandes de l’interface de ligne de commande aio](https://github.com/adobe/aio-cli#usage)

[Plug-in d’interface de ligne de commande Adobe I/O Runtime pour les interactions avec les environnements de développement rapide AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Plug-in d’interface de ligne de commande aio de Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)
