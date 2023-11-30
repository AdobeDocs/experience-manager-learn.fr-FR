---
title: Configurer un environnement de développement local pour l’extensibilité Assets Compute
description: 'Le développement de programmes de travail Assets Compute, qui sont des applications JavaScript Node.js, nécessite des outils de développement spécifiques dont AEM n’est pas coutumier : Node.js, divers modules npm, ainsi que Docker Desktop et Microsoft Visual Studio Code.'
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 100%

---

# Configurer l’environnement de développement local

Les projets Assets Compute ne peuvent pas être intégrés à l’exécution AEM locale fournie par le SDK AEM et sont développés à l’aide de leur propre chaîne d’outils, distincte de celle requise par les applications en fonction de l’archétype de projet Maven.

Pour étendre les microservices Assets Compute, les outils suivants doivent être installés sur la machine de développement locale.

## Instructions de configuration abrégées.

Vous trouverez ci-dessous des instructions de configuration abrégées. Ces outils de développement sont décrits dans les sections ci-dessous.

1. [Installez Docker Desktop](https://www.docker.com/products/docker-desktop) et procédez à l’extraction des images Docker requises :

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installer Visual Studio Code](https://code.visualstudio.com/download)
1. [Installer Node.js 10 ou version ultérieure](../../local-development-environment/development-tools.md#node-js)
1. Installez les modules npm requis et les plug-ins CLI d’Adobe I/O à partir de la ligne de commande :

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Pour plus d’informations sur les instructions d’installation abrégées, consultez les sections ci-dessous.

## Installer Visual Studio Code{#vscode}

[Microsoft Visual Studio Code](https://code.visualstudio.com/download) est utilisé à des fins de développement et de débogage des programmes de travail Assets Compute. Bien que d’autres [IDE compatibles avec JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) puissent participer au développement du programme de travail, seul Visual Studio Code permet de [déboguer](../test-debug/debug.md) le programme de travail Assets Compute.

Ce tutoriel utilise Visual Studio Code afin d’offrir la meilleure expérience de développement et tirer pleinement parti d’Asset Compute.

## Installer Docker Desktop{#docker}

Téléchargez et installez la dernière version stable de [Docker Desktop](https://www.docker.com/products/docker-desktop), nécessaire pour [tester](../test-debug/test.md) et [déboguer](../test-debug/debug.md) localement des projets Assets Compute.

Une fois Docker Desktop installé, lancez l’application et installez les images Docker suivantes à partir de la ligne de commande :

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Les développeurs et développeuses Windows doivent s’assurer d’utiliser des conteneurs Linux pour les images ci-dessus. Suivez la procédure d’utilisation des conteneurs Linux dans la section [Documentation de Docker pour Windows](https://docs.docker.com/docker-for-windows/).

## Installer Node.js (et npm){#node-js}

Les programmes de travail Assets Compute sont basés sur [Node.js](https://nodejs.org/) et nécessitent donc Node.js 10 ou version ultérieure (et npm) pour le développement et la création.

+ [Installez Node.js (et npm)](../../local-development-environment/development-tools.md#node-js) comme pour le développement AEM traditionnel.

## Installer l’interface de ligne de commande Adobe I/O{#aio}

[Installez l’interface de ligne de commande d’Adobe I/O](../../local-development-environment/development-tools.md#aio-cli), ou __aio__, qui est un module npm de ligne de commande (CLI) et facilite l’utilisation et l’interaction avec les technologies d’Adobe I/O. Il permet de générer et de développer localement des programmes de travail Assets Compute personnalisés.

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_L’interface de ligne de commande d’Adobe I/O version 7.1.0 est requise. Les versions ultérieures de l’interface de ligne de commande d’Adobe I/O ne sont pas prises en charge pour le moment._


## Installer le plug-in Asset Compute d’interface de ligne de commande d’Adobe I/O{#aio-asset-compute}

[Plug-in Asset Compute d’interface de ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installer wskdebug{#wskdebug}

Téléchargez et installez le module npm de [débogage Apache OpenWhisk](https://www.npmjs.com/package/@openwhisk/wskdebug) pour faciliter le débogage local des programmes de travail Assets Compute.

_Visual Studio Code 1.48.x ou version ultérieure est requis pour exécuter [wskdebug](#wskdebug)._

```
$ npm install -g @openwhisk/wskdebug
```

## Installer ngrok{#ngrok}

Téléchargez et installez le module npm [ngrok](https://www.npmjs.com/package/ngrok), qui permet au public d’accéder à votre machine de développement locale, afin de faciliter le débogage local des programmes de travail Assets Compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
