---
title: Configurer un environnement de développement local pour l'extensibilité des Assets compute
description: Le développement de travailleurs d'Asset compute, qui sont des applications JavaScript Node.js, nécessite des outils de développement spécifiques qui diffèrent du développement AEM traditionnel, allant de Node.js et de divers modules npm à Docker Desktop et à Microsoft Visual Studio Code.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 1%

---


# Configuration de l&#39;environnement de développement local

Les projets d&#39;Asset compute d&#39;Adobe ne peuvent pas être intégrés au runtime d&#39;AEM local fourni par AEM SDK et sont développés à l&#39;aide de leur propre chaîne d&#39;outils, distincte de celle requise par les applications de l&#39; Maven en fonction de l&#39;archétype du projet.

Pour étendre les microservices d&#39;Asset compute, les outils suivants doivent être installés sur le développeur local.

## Instructions de configuration abrégées

Vous trouverez ci-dessous des instructions de configuration abrégées. Vous trouverez ci-dessous des informations détaillées sur ces outils de développement dans des sections distinctes.

1. [Installez Docker ](https://www.docker.com/products/docker-desktop) Desktopet extrayez les images Docker requises :

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:latest
   ```

1. [Installation du code Visual Studio](https://code.visualstudio.com/download)
1. [Installation de Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installez les modules npm requis et les modules CLI Adobe I/O à partir de la ligne de commande :

   ```
   $ npm i -g @adobe/aio-cli @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Pour plus d&#39;informations sur les instructions d&#39;installation abrégées, lisez les sections ci-dessous.

## Installer le code Visual Studio{#vscode}

[Le ](https://code.visualstudio.com/download) code Microsoft Visual Studio est utilisé pour le développement et le débogage des travailleurs d&#39;Asset compute. Bien que d&#39;autres [IDE compatibles JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) puissent être utilisés pour développer le programme de travail, seul le code Visual Studio peut être intégré à [debug](../test-debug/debug.md) Asset compute worker.

_Visual Studio Code 1.48.x+ est requis pour que  [](#wskdebug) wskdebugto fonctionne._

Ce didacticiel suppose l&#39;utilisation du code Visual Studio car il fournit la meilleure expérience de développement pour étendre l&#39;Asset compute.

## Installer Docker Desktop{#docker}

Téléchargez et installez les derniers projets d&#39;Asset compute [Docker Desktop](https://www.docker.com/products/docker-desktop) stables, car cela est nécessaire pour [tester](../test-debug/test.md) et [déboguer](../test-debug/debug.md) localement.

Après avoir installé Docker Desktop, début-le et installez les images Docker suivantes à partir de la ligne de commande :

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Les développeurs sur les ordinateurs Windows doivent s&#39;assurer qu&#39;ils utilisent des conteneurs Linux pour les images ci-dessus. Les étapes pour passer aux conteneurs Linux sont décrites dans la documentation [Docker for Windows](https://docs.docker.com/docker-for-windows/).

## Installer Node.js (et npm){#node-js}

Les travailleurs d’Asset compute sont basés sur [Node.js](https://nodejs.org/) et nécessitent donc Node.js 10+ (et npm) pour le développement et la création.

+ [Installez Node.js (et npm)](../../local-development-environment/development-tools.md#node-js) de la même manière que pour le développement AEM traditionnel.

## Installer Adobe I/O CLI{#aio}

[Installez l&#39;interface de ligne de commande](../../local-development-environment/development-tools.md#aio-cli) Adobe I/O, ou module npm  ____ aiois an command-line (CLI) qui facilite l&#39;utilisation et l&#39;interaction avec les technologies Adobe I/O, et qui est utilisé pour générer et développer localement des travailleurs d&#39;Asset compute personnalisés.

```
$ npm install -g @adobe/aio-cli
```

## Installez le module Adobe I/O CLI Asset compute plugin{#aio-asset-compute}

[Module externe d’Asset compute de l’interface de ligne de commande ](https://github.com/adobe/aio-cli-plugin-asset-compute) Adobe I/O

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installer wskdebug{#wskdebug}

Téléchargez et installez le module de débogage [Apache OpenWhisk](https://www.npmjs.com/package/@openwhisk/wskdebug) npm pour faciliter le débogage local des travailleurs d&#39;Asset compute.

_Visual Studio Code 1.48.x+ est requis pour que  [](#wskdebug) wskdebugto fonctionne._

```
$ npm install -g @openwhisk/wskdebug
```

## Installer ngrok{#ngrok}

Téléchargez et installez le module [ngrok](https://www.npmjs.com/package/ngrok) npm, qui permet au public d&#39;accéder à votre machine de développement locale, afin de faciliter le débogage local des employés d&#39;Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
