---
title: Configuration d’un environnement de développement local pour l’extensibilité des Assets compute
description: Le développement d’objets Worker Asset compute, qui sont des applications JavaScript Node.js, nécessite des outils de développement spécifiques qui diffèrent du développement AEM traditionnel, allant de Node.js et de divers modules npm à Docker Desktop et Microsoft Visual Studio Code.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6266
thumbnail: KT-6266.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 162e10e5-fcb0-4f16-b6d1-b951826209d9
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 2%

---

# Configuration de l’environnement de développement local

Les projets Adobe Asset compute ne peuvent pas être intégrés au runtime AEM local fourni par le SDK AEM et sont développés à l’aide de leur propre chaîne d’outils, distincte de celle requise par les applications  en fonction de l’archétype de projet Maven.

Pour étendre les microservices Asset compute, les outils suivants doivent être installés sur la machine locale du développeur.

## Instructions de configuration abrégées

Vous trouverez ci-dessous des instructions de configuration abrégées. Les détails de ces outils de développement sont décrits dans des sections discrètes ci-dessous.

1. [Installez Docker ](https://www.docker.com/products/docker-desktop) Desktopet extrayez les images Docker requises :

   ```
   $ docker pull openwhisk/action-nodejs-v12:latest
   $ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
   ```

1. [Installation du code Visual Studio](https://code.visualstudio.com/download)
1. [Installation de Node.js 10+](../../local-development-environment/development-tools.md#node-js)
1. Installez les modules npm requis et les modules CLI d’Adobe I/O à partir de la ligne de commande :

   ```
   $ npm i -g @adobe/aio-cli@7.1.0 @openwhisk/wskdebug ngrok --unsafe-perm=true \
   && aio plugins:install @adobe/aio-cli-plugin-asset-compute
   ```

Pour plus d’informations sur les instructions d’installation abrégées, consultez les sections ci-dessous.

## Installation du code Visual Studio{#vscode}

[Le ](https://code.visualstudio.com/download) code Microsoft Visual Studio est utilisé pour le développement et le débogage des objets Worker Asset compute. Bien que d’autres [IDE compatibles avec JavaScript](../../local-development-environment/development-tools.md#set-up-the-development-ide) puissent être utilisés pour développer le programme de travail, seul le code Visual Studio peut être intégré à [debug](../test-debug/debug.md) Asset compute worker.

Ce tutoriel suppose l’utilisation de Visual Studio Code, car il fournit la meilleure expérience de développement pour étendre l’Asset compute.

## Installation de Docker Desktop{#docker}

Téléchargez et installez la dernière version de [Docker Desktop](https://www.docker.com/products/docker-desktop), car cela est nécessaire pour [tester](../test-debug/test.md) et [déboguer](../test-debug/debug.md) Asset compute les projets localement.

Après avoir installé Docker Desktop, démarrez-le et installez les images Docker suivantes à partir de la ligne de commande :

```
$ docker pull openwhisk/action-nodejs-v12:latest
$ docker pull adobeapiplatform/adobe-action-nodejs-v12:3.0.22
```

Les développeurs sur les machines Windows doivent s’assurer qu’ils utilisent des conteneurs Linux pour les images ci-dessus. Les étapes de basculement vers des conteneurs Linux sont décrites dans la [documentation Docker pour Windows](https://docs.docker.com/docker-for-windows/).

## Installation de Node.js (et npm){#node-js}

Les objets Worker Asset compute sont basés sur [Node.js](https://nodejs.org/) et nécessitent donc Node.js 10+ (et npm) pour le développement et la création.

+ [Installez Node.js (et npm)](../../local-development-environment/development-tools.md#node-js)  de la même manière que pour le développement AEM traditionnel.

## Installation de l’interface de ligne de commande Adobe I/O{#aio}

[Installez l’interface de ligne de commande ](../../local-development-environment/development-tools.md#aio-cli) ou le module npm  ____ aiois an command line (CLI) Adobe I/O qui facilite l’utilisation et l’interaction avec les technologies d’Adobe I/O. Il est utilisé pour générer et développer localement des agents d’Asset compute personnalisés.

```
$ npm install -g @adobe/aio-cli@7.1.0
```

_La version 7.1.0 de l’interface de ligne de commande d’Adobe I/O est requise. Les versions ultérieures de l’interface de ligne de commande d’Adobe I/O ne sont pas prises en charge pour le moment._


## Installation du module Adobe I/O CLI Asset compute{#aio-asset-compute}

[Module externe d’Asset compute de la ligne de commande d’Adobe I/O](https://github.com/adobe/aio-cli-plugin-asset-compute)

```
$ aio plugins:install @adobe/aio-cli-plugin-asset-compute
```

## Installer wskdebug{#wskdebug}

Téléchargez et installez le module npm [Apache OpenWhisk debug](https://www.npmjs.com/package/@openwhisk/wskdebug) pour faciliter le débogage local des objets Worker Asset compute.

_Visual Studio Code 1.48.x+ est requis pour que  [](#wskdebug) wskdebugto fonctionne._

```
$ npm install -g @openwhisk/wskdebug
```

## Installer ngrok{#ngrok}

Téléchargez et installez le module npm [ngrok](https://www.npmjs.com/package/ngrok), qui permet au public d’accéder à votre machine de développement locale, afin de faciliter le débogage local des objets Worker Asset compute.

```
$ npm install -g ngrok --unsafe-perm=true
```
