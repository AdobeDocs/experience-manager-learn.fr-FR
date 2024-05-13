---
title: Configuration du développement local
description: Découvrez comment configurer un environnement de développement local pour Universal Editor afin que vous puissiez modifier le contenu d’un exemple d’application React.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
source-git-commit: 14767141348d3d56c154704cc21d39722bb67aec
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 3%

---


# Configuration du développement local

Découvrez comment configurer un environnement de développement local pour modifier le contenu d’une application React à l’aide d’AEM Universal Editor.

## Conditions préalables

Pour suivre ce tutoriel, vous devez suivre les étapes ci-après :

- Compétences de base en HTML et JavaScript.
- Les outils suivants doivent être installés localement :
   - [Node.js](https://nodejs.org/fr/download/)
   - [Git](https://git-scm.com/downloads)
   - un IDE ou un éditeur de code, tel que [Visual Studio Code](https://code.visualstudio.com/)
- Téléchargez et installez les éléments suivants :
   - [AEM SDK AS A CLOUD SERVICE](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime#download-the-aem-as-a-cloud-service-sdk): contient le fichier Quickstart Jar utilisé pour exécuter AEM Auteur et Publier localement à des fins de développement.
   - [Service Universal Editor](https://experienceleague.adobe.com/en/docs/experience-cloud/software-distribution/home): copie locale du service Universal Editor, il comporte un sous-ensemble de fonctionnalités et peut être téléchargé à partir du portail de distribution de logiciels.
   - [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy#local-ssl-proxy): proxy HTTP SSL local simple utilisant un certificat auto-signé pour le développement local. AEM Universal Editor requiert l’URL HTTPS de l’application React pour le charger dans l’éditeur.

## Configuration locale

Pour configurer l’environnement de développement local, procédez comme suit :

### SDK AEM

Pour fournir le contenu de l’application WKND Teams React, installez les modules suivants dans le SDK AEM local.

- [WKND Teams - Package de contenu](./assets/basic-tutorial-solution.content.zip): contient les modèles de fragment de contenu, les fragments de contenu et les requêtes GraphQL persistantes.
- [Équipes WKND - Package de configuration](./assets/basic-tutorial-solution.ui.config.zip): contient les configurations CORS (Cross-Origin Resource Sharing) et Token Authentication Handler. La norme CORS facilite les propriétés web non AEM pour effectuer des appels côté client basés sur un navigateur vers AEM API GraphQL et le gestionnaire d’authentification des jetons est utilisé pour authentifier chaque demande à l’.

  ![Équipes WKND - Packages](./assets/wknd-teams-packages.png)

### Application React

Pour configurer l’application WKND Teams React, procédez comme suit :

1. Cloner le [Application WKND React pour les équipes](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) de la `basic-tutorial` branche de solution.

   ```bash
   $ git clone -b solution/basic-tutorial git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Accédez au `basic-tutorial` et ouvrez-le dans votre éditeur de code.

   ```bash
   $ cd aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

1. Installez les dépendances et démarrez l’application React.

   ```bash
   $ npm install
   $ npm start
   ```

1. Ouvrez l’application WKND Teams React dans votre navigateur à l’adresse [http://localhost:3000](http://localhost:3000). Elle affiche la liste des membres de l’équipe et leurs détails. Le contenu de l’application React est fourni par le SDK d’AEM local à l’aide des API GraphQL (`/graphql/execute.json/my-project/all-teams`), que vous pouvez vérifier à l’aide de l’onglet réseau du navigateur.

   ![WKND Teams - application React](./assets/wknd-teams-react-app.png)

### Service d’éditeur universel

Pour configurer la variable **local** Service Universal Editor, procédez comme suit :

1. Téléchargez la dernière version du service Universal Editor à partir du [Portail de distribution de logiciels](https://experience.adobe.com/downloads).

   ![Distribution logicielle - Service d’éditeur universel](./assets/universal-editor-service.png)

1. Extrayez le fichier ZIP téléchargé et copiez le fichier `universal-editor-service.cjs` vers un nouveau répertoire nommé `universal-editor-service`.

   ```bash
   $ unzip universal-editor-service-vproduction-<version>.zip
   $ mkdir universal-editor-service
   $ cp universal-editor-service.cjs universal-editor-service
   ```

1. Créer `.env` dans le fichier `universal-editor-service` et ajoutez les variables d’environnement suivantes :

   ```bash
   # The port on which the Universal Editor service runs
   EXPRESS_PORT=8000
   # Disable SSL verification
   NODE_TLS_REJECT_UNAUTHORIZED=0
   ```

1. Démarrez le service local Universal Editor.

   ```bash
   $ cd universal-editor-service
   $ node universal-editor-service.cjs
   ```

La commande ci-dessus lance le service Universal Editor sur le port `8000` et vous devriez voir la sortie suivante :

```bash
Either no private key or certificate was set. Starting as HTTP server
Universal Editor Service listening on port 8000 as HTTP Server
```

### Proxy HTTP SSL local

AEM Universal Editor exige que l’application React soit diffusée via HTTPS. Configurez un proxy HTTP SSL local qui utilise un certificat auto-signé pour le développement local.

Suivez les étapes ci-dessous pour configurer le proxy HTTP SSL local et servir le SDK AEM et le service Universal Editor via HTTPS :

1. Installez le `local-ssl-proxy` module global.

   ```bash
   $ npm install -g local-ssl-proxy
   ```

1. Démarrez deux instances du proxy HTTP SSL local pour les services suivants :

   - AEM SDK proxy HTTP SSL local sur le port `8443`.
   - proxy HTTP SSL local du service Universal Editor sur le port `8001`.

   ```bash
   # AEM SDK local SSL HTTP proxy on port 8443
   $ local-ssl-proxy --source 8443 --target 4502
   
   # Universal Editor service local SSL HTTP proxy on port 8001
   $ local-ssl-proxy --source 8001 --target 8000
   ```

### Mettre à jour l’application React pour utiliser HTTPS

Pour activer HTTPS pour l’application WKND Teams React, procédez comme suit :

1. Arrêtez le React en appuyant sur `Ctrl + C` dans le terminal.
1. Mettez à jour le `package.json` pour inclure le fichier `HTTPS=true` de la variable d’environnement `start` script.

   ```json
   "scripts": {
       "start": "HTTPS=true react-scripts start",
       ...
   }
   ```

1. Mettez à jour le `REACT_APP_HOST_URI` dans le `.env.development` pour utiliser le protocole HTTPS et le port proxy HTTP SSL local du SDK AEM.

   ```bash
   REACT_APP_HOST_URI=https://localhost:8443
   ...
   ```

1. Mettez à jour le `../src/proxy/setupProxy.auth.basic.js` pour utiliser des paramètres SSL décontractés à l’aide de `secure: false` .

   ```javascript
   ...
   module.exports = function(app) {
   app.use(
       ['/content', '/graphql'],
       createProxyMiddleware({
       target: REACT_APP_HOST_URI,
       changeOrigin: true,
       secure: false, // Ignore SSL certificate errors
       // pass in credentials when developing against an Author environment
       auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`
       })
   );
   };
   ```

1. Démarrez l’application React.

   ```bash
   $ npm start
   ```

## Vérification de la configuration

Après avoir configuré l’environnement de développement local en suivant les étapes ci-dessus, nous allons vérifier la configuration.

### Vérification locale

Assurez-vous que les services suivants s’exécutent localement sur HTTPS. Vous devrez peut-être accepter l’avertissement de sécurité dans le navigateur pour le certificat auto-signé :

1. Application WKND React pour les équipes [https://localhost:3000](https://localhost:3000)
1. AEM SDK sur [https://localhost:8443](https://localhost:8443)
1. Service d’éditeur universel activé [https://localhost:8001](https://localhost:8001)

### Charger l’application WKND Teams React dans Universal Editor

Chargons l’application WKND Teams React dans Universal Editor pour vérifier la configuration :

1. Ouvrez Universal Editor https://experience.adobe.com/#/aem/editor dans votre navigateur. Si vous y êtes invité, connectez-vous à l’aide de votre Adobe ID.

1. Saisissez l’URL de l’application WKND Teams React dans le champ de saisie URL du site de l’éditeur universel, puis cliquez sur `Open`.

   ![Éditeur universel - URL du site](./assets/universal-editor-site-url.png)

1. L’application WKND Teams React se charge dans l’éditeur universel **mais vous ne pouvez pas encore modifier le contenu.**. Vous devez utiliser l’application React pour activer l’édition de contenu à l’aide d’Universal Editor.

   ![Éditeur universel - Application WKND Teams React](./assets/universal-editor-wknd-teams.png)


## Étape suivante

Découvrez comment [instrumenter l’application React pour modifier le contenu](./instrument-to-edit-content.md).
