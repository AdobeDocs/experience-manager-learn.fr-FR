---
title: Configuration d’un environnement de développement local pour l’extensibilité Asset Compute
description: Le développement des ressources informatiques, qui sont des applications JavaScript Node.js, nécessite des outils de développement spécifiques qui diffèrent du développement AEM traditionnel, allant de Node.js et de divers modules npm à Docker Desktop et à Microsoft Visual Studio Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 53e4235c55d890765e9f13ffeb37a2c805fb307b
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# Configuration de l&#39;environnement de développement local

Les applications Adobe Asset Compute ne peuvent pas être intégrées au runtime AEM local fourni par AEM SDK et sont développées à l&#39;aide de leur propre chaîne d&#39;outils, distincte de celle requise par les applications de l&#39; Maven en fonction de l&#39;archétype du projet.

Pour étendre les microservices Asset Compute, les outils suivants doivent être installés sur l’ordinateur de développement local.

## Instructions de configuration abrégées

Vous trouverez ci-dessous des instructions de configuration abrégées. Vous trouverez ci-dessous des informations détaillées sur ces outils de développement dans des sections distinctes.

1. [Installez Docker Desktop](https://www.docker.com/products/docker-desktop) et extrayez les images Docker requises :

   ```
   $ docker pull openwhisk/action-nodejs-v10:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installation du code Visual Studio](https://code.visualstudio.com/download)
1. [Installation de Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installez les modules npm requis et les modules CLI d&#39;E/S Adobe à partir de la ligne de commande :

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

## Installation du code Visual Studio{#vscode}

[Le code](https://code.visualstudio.com/download) Microsoft Visual Studio est utilisé pour le développement et le débogage des applications Asset Compute. Bien que d&#39;autres environnements IDE [compatibles](../../local-development-environment/development-tools.md#set-up-the-development-ide) JavaScript puissent être utilisés pour développer l&#39;application, seul le code Visual Studio peut être intégré pour [déboguer](../test-debug/debug.md) les applications Asset Compute.

_Visual Studio Code 1.48.x+ est requis pour que[wskdebug](#wskdebug)fonctionne._

Ce didacticiel suppose l&#39;utilisation du code Visual Studio car il fournit la meilleure expérience de développement pour l&#39;extension d&#39;Asset Compute.

## Installer le bureau du Docker{#docker}

Téléchargez et installez la dernière version stable du [Docker Desktop](https://www.docker.com/products/docker-desktop), car cela est nécessaire pour [tester](../test-debug/test.md) et [déboguer](../test-debug/debug.md) localement les projets Asset Compute.

Après avoir installé Docker Desktop, début-le et installez les images Docker suivantes à partir de la ligne de commande :

```
$ docker pull openwhisk/action-nodejs-v10:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Les développeurs sur les ordinateurs Windows doivent s&#39;assurer qu&#39;ils utilisent des conteneurs Linux pour les images ci-dessus. Les étapes pour passer aux conteneurs Linux sont décrites dans la documentation [du](https://docs.docker.com/docker-for-windows/)Docker pour Windows.

## Installation de Node.js (et npm){#node-js}

Les programmes de travail Asset Compute sont des applications [Node.js](https://nodejs.org/) et nécessitent donc Node.js 10+ (et npm) pour le développement et la création.

+ [Installez Node.js (et npm)](../../local-development-environment/development-tools.md#node-js) de la même manière que pour le développement AEM traditionnel.

## Install Adobe I/O CLI{#aio}

[Installez l&#39;interface de ligne de commande](../../local-development-environment/development-tools.md#aio-cli)d&#39;Adobe E/S ou __aio__ est un module Npm de ligne de commande (CLI) qui facilite l&#39;utilisation et l&#39;interaction avec les technologies d&#39;E/S d&#39;Adobe et qui est utilisé pour générer et développer localement des employés personnalisés d&#39;Asset Compute.

```
$ npm install -g @adobe/aio-cli
```

## Installation du module externe Adobe E/S CLI Asset Compute{#aio-asset-compute}

Plug-in [Adobe E/S CLI Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installer wskdebug{#wskdebug}

Téléchargez et installez le module [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm pour faciliter le débogage local des applications Asset Compute.

_Visual Studio Code 1.48.x+ est requis pour que[wskdebug](#wskdebug)fonctionne._

```
$ npm install -g @openwhisk/wskdebug
```

## Installer ngrok{#ngrok}

Téléchargez et installez le module [npm ngrok](https://www.npmjs.com/package/ngrok) , qui permet au public d’accéder à votre machine de développement locale, afin de faciliter le débogage local des applications Asset Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
