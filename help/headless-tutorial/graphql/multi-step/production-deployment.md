---
title: Déploiement de production à l’aide d’un service de publication AEM - Prise en main d’AEM sans affichage - GraphQL
description: Découvrez les services d’auteur et de publication AEM et le modèle de déploiement recommandé pour les applications sans interface utilisateur graphique. Dans ce tutoriel, apprenez à utiliser des variables d’environnement pour modifier dynamiquement un point d’entrée GraphQL en fonction de l’environnement cible. Découvrez comment configurer correctement les AEM pour le partage des ressources cross-origin (CORS).
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2360'
ht-degree: 7%

---

# Déploiement de production avec un service de publication AEM

Dans ce tutoriel, vous allez configurer un environnement local pour simuler le contenu distribué d’une instance d’auteur à une instance de publication. Vous allez également générer la version de production d’une application React configurée pour utiliser du contenu de l’environnement de publication AEM à l’aide des API GraphQL. Ce faisant, vous apprendrez à utiliser efficacement les variables d’environnement et à mettre à jour les configurations CORS AEM.

## Prérequis

Ce tutoriel fait partie d’un tutoriel en plusieurs parties. On suppose que les étapes décrites dans les parties précédentes ont été terminées.

## Objectifs

Découvrez comment :

* Comprendre l’architecture de création et de publication AEM.
* Découvrez les bonnes pratiques de gestion des variables d’environnement.
* Découvrez comment configurer correctement les AEM pour le partage des ressources cross-origin (CORS).

## Modèle de déploiement de la publication de l’auteur {#deployment-pattern}

Un environnement d’AEM complet est constitué d’un auteur, d’une publication et d’un Dispatcher. Le service Auteur permet aux utilisateurs internes de créer, gérer et prévisualiser du contenu. Le service de publication est considéré comme l’environnement &quot;En ligne&quot; et est généralement ce avec lequel les utilisateurs finaux interagissent. Le contenu, après avoir été modifié et approuvé sur le service Auteur, est distribué au service Publication.

Le modèle de déploiement le plus courant avec les applications découplées AEM est de connecter la version de production de l’application à un service de publication AEM.

![Modèle de déploiement de haut niveau](assets/publish-deployment/high-level-deployment.png)

Le diagramme ci-dessus illustre ce schéma de déploiement commun.

1. Un **auteur de contenu** utilise le service de création d’AEM pour créer, modifier et gérer du contenu.
2. **L’auteur du contenu** et d’autres utilisateurs internes peuvent prévisualiser le contenu directement sur le service d’auteur. Une version Aperçu de l’application peut être configurée pour se connecter au service Auteur.
3. Une fois que le contenu a été approuvé, il peut être **publié** sur le service de publication AEM.
4. **Les** utilisateurs finaux interagissent avec la version de production de l’application. L’application de production se connecte au service de publication et utilise les API GraphQL pour demander et utiliser du contenu.

Le tutoriel simule le déploiement ci-dessus en ajoutant une instance AEM Publish à la configuration actuelle. Dans les chapitres précédents, React App a servi de prévisualisation en se connectant directement à l’instance d’auteur. Une version de production de l’application React sera déployée sur un serveur Node.js statique qui se connecte à la nouvelle instance de publication.

Au final, trois serveurs locaux seront en cours d’exécution :

* http://localhost:4502 - Instance de création
* http://localhost:4503 - Instance de publication
* http://localhost:5000 - React App en mode de production, connexion à l’instance de publication.

## Installation AEM SDK - mode de publication {#aem-sdk-publish}

Actuellement, une instance en cours d’exécution du SDK est en mode **Auteur**. Le SDK peut également être démarré en mode **Publier** pour simuler un environnement de publication AEM.

Vous trouverez un guide plus détaillé de configuration d’un environnement de développement local [ici](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr#local-development-environment-set-up).

1. Sur votre système de fichiers local, créez un dossier dédié pour installer l’instance de publication, c’est-à-dire `~/aem-sdk/publish`.
1. Copiez le fichier JAR Quickstart utilisé pour l’instance d’auteur dans les chapitres précédents et collez-le dans le répertoire `publish`. Vous pouvez également accéder au [portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html), télécharger le dernier SDK et extraire le fichier JAR de démarrage rapide.
1. Renommez le fichier jar en `aem-publish-p4503.jar`.

   La chaîne `publish` indique que le fichier Quickstart jar démarre en mode de publication. `p4503` spécifie que le serveur de démarrage rapide s’exécute sur le port 4503.

1. Ouvrez une nouvelle fenêtre de terminal et accédez au dossier contenant le fichier jar. Installez et démarrez l’instance AEM :

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Saisissez un mot de passe administrateur `admin`. Tout mot de passe administrateur est acceptable, mais il est recommandé d’utiliser la valeur par défaut pour le développement local afin d’éviter des configurations supplémentaires.
1. Une fois l’installation de l’instance AEM terminée, une nouvelle fenêtre de navigateur s’ouvre à l’adresse [http://localhost:4503/content.html](http://localhost:4503/content.html).

   Une page 404 Not Found doit être renvoyée. Il s’agit d’une nouvelle instance AEM et aucun contenu n’a été installé.

## Installation d’exemples de contenu et de points d’entrée GraphQL {#wknd-site-content-endpoints}

Tout comme sur l’instance d’auteur, les points d’entrée GraphQL doivent être activés pour l’instance de publication et un exemple de contenu est nécessaire. Installez ensuite le site de référence WKND sur l’instance de publication.

1. Téléchargez le dernier AEM compilé pour le site WKND : [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Veillez à télécharger la version standard compatible avec AEM en tant que Cloud Service et **pas** la version `classic`.

1. Connectez-vous à l’instance de publication en accédant directement à : [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) avec le nom d’utilisateur `admin` et le mot de passe `admin`.
1. Ensuite, accédez au gestionnaire de modules à l’adresse [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Cliquez sur **Télécharger le package** et sélectionnez le package WKND téléchargé à l’étape précédente. Cliquez sur **Installer** pour installer le package.
1. Après l’installation du package, le site de référence WKND est désormais disponible à l’adresse [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Déconnectez-vous en tant qu’utilisateur `admin` en cliquant sur le bouton &quot;Se déconnecter&quot; dans la barre de menus.

   ![Site de référence de déconnexion WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   Contrairement à l’instance d’auteur AEM, les instances de publication AEM optent par défaut pour un accès anonyme en lecture seule. Nous voulons simuler l&#39;expérience d&#39;un utilisateur anonyme lors de l&#39;exécution de l&#39;application React.

## Mise à jour des variables d’environnement pour qu’elles pointent l’instance de publication {#react-app-publish}

Mettez ensuite à jour les variables d’environnement utilisées par l’application React pour pointer vers l’instance de publication. L’application React doit **se connecter uniquement** à l’instance de publication en mode de production.

Ajoutez ensuite un nouveau fichier `.env.production.local` pour simuler l’expérience de production.

1. Ouvrez l’application WKND GraphQL React dans votre IDE.

1. Sous `aem-guides-wknd-graphql/react-app`, ajoutez un fichier nommé `.env.production.local`.
1. Remplissez `.env.production.local` avec les éléments suivants :

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Ajout d’un nouveau fichier de variables d’environnement](assets/publish-deployment/env-production-local-file.png)

   L’utilisation de variables d’environnement facilite le basculement du point d’entrée GraphQL entre un environnement de création ou de publication sans ajouter de logique supplémentaire dans le code de l’application. Vous trouverez plus d’informations sur les [variables d’environnement personnalisées pour React ici](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Notez qu’aucune information d’authentification n’est incluse, car les environnements de publication permettent par défaut un accès anonyme au contenu.

## Déploiement d’un serveur de noeud statique {#static-server}

L’application React peut être lancée à l’aide du serveur webpack, mais cela est réservé au développement. Ensuite, simulez un déploiement en production en utilisant [serve](https://github.com/vercel/serve) pour héberger une version en production de l’application React à l’aide de Node.js.

1. Ouvrez une nouvelle fenêtre de terminal et accédez au répertoire `aem-guides-wknd-graphql/react-app`

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Installez [serve](https://github.com/vercel/serve) avec la commande suivante :

   ```shell
   $ npm install serve --save-dev
   ```

1. Ouvrez le fichier `package.json` dans `react-app/package.json`. Ajoutez un script nommé `serve` :

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   Le script `serve` effectue deux actions. Tout d’abord, une version de production de l’application React est générée. Deuxièmement, le serveur Node.js démarre et utilise la version de production.

1. Revenez au terminal et saisissez la commande pour démarrer le serveur statique :

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. Ouvrez un nouveau navigateur et accédez à [http://localhost:5000/](http://localhost:5000/). Vous devriez voir l’application React diffusée.

   ![React App Served](assets/publish-deployment/react-app-served-port5000.png)

   Notez que la requête GraphQL fonctionne sur la page d’accueil. Inspect de la demande **XHR** à l’aide de vos outils de développement. Notez que le POST GraphQL se trouve sur l’instance de publication à l’adresse `http://localhost:4503/content/graphql/global/endpoint.json`.

   Cependant, toutes les images sont cassées sur la page d&#39;accueil !

1. Cliquez sur l’une des pages Détails de l’aventure.

   ![Erreur de détails de l’aventure](assets/publish-deployment/adventure-detail-error.png)

   Observez qu’une erreur GraphQL est générée pour `adventureContributor`. Dans les exercices suivants, les images rompues et les problèmes `adventureContributor` sont résolus.

## Références d’image absolue {#absolute-image-references}

Les images semblent rompues, car l’attribut `<img src` est défini sur un chemin relatif et pointe vers le serveur statique Node à `http://localhost:5000/`. À la place, ces images doivent pointer vers l’instance de publication AEM. Il existe plusieurs solutions possibles. Lors de l’utilisation du serveur de développement webpack, le fichier `react-app/src/setupProxy.js` configure un proxy entre le serveur webpack et l’instance d’auteur AEM pour toutes les demandes à `/content`. Une configuration proxy peut être utilisée dans un environnement de production, mais doit être configurée au niveau du serveur web. Par exemple, [Module proxy d’Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

L’application peut être mise à jour afin d’inclure une URL absolue à l’aide de la variable d’environnement `REACT_APP_HOST_URI`. Utilisez plutôt une fonctionnalité de l’API GraphQL AEM pour demander une URL absolue à l’image.

1. Arrêtez le serveur Node.js.
1. Revenez à l’IDE et ouvrez le fichier `Adventures.js` à l’adresse `react-app/src/components/Adventures.js`.
1. Ajoutez la propriété `_publishUrl` à la `ImageRef` dans la balise `allAdventuresQuery` :

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` et  `_authorUrl` sont des valeurs intégrées à l’ `ImageRef` objet afin de faciliter l’inclusion des url absolues.

1. Répétez les étapes ci-dessus pour modifier la requête utilisée dans la fonction `filterQuery(activity)` afin d’inclure la propriété `_publishUrl`.
1. Modifiez le composant `AdventureItem` à l’emplacement `function AdventureItem(props)` pour référencer la propriété `_publishUrl` au lieu de la propriété `_path` lors de la création de la balise `<img src=''>` :

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Ouvrez le fichier `AdventureDetail.js` dans `react-app/src/components/AdventureDetail.js`.
1. Répétez les mêmes étapes pour modifier la requête GraphQL et ajouter la propriété `_publishUrl` pour l’aventure.

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. Modifiez les deux balises `<img>` pour l’image Principal aventure et la référence Image du contributeur dans `AdventureDetail.js` :

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. Revenez au terminal et démarrez le serveur statique :

   ```shell
   $ npm run serve
   ```

1. Accédez à [http://localhost:5000/](http://localhost:5000/) et observez que les images apparaissent et que l’attribut `<img src''>` pointe vers `http://localhost:4503`.

   ![Correction des images rompues](assets/publish-deployment/broken-images-fixed.png)

## Simulation de la publication de contenu {#content-publish}

Souvenez-vous qu’une erreur GraphQL est générée pour `adventureContributor` lorsqu’une page Détails de l’aventure est demandée. Le modèle de fragment de contenu **Contributor** n’existe pas encore sur l’instance de publication. Les mises à jour apportées au **modèle de fragment de contenu Adventure** ne sont pas non plus disponibles sur l’instance de publication. Ces modifications ont été apportées directement à l’instance d’auteur et doivent être distribuées à l’instance de publication.

C’est quelque chose à prendre en compte lors du déploiement de nouvelles mises à jour d’une application qui repose sur des mises à jour d’un fragment de contenu ou d’un modèle de fragment de contenu.

Ensuite, permet de simuler la publication de contenu entre les instances d’auteur et de publication locales.

1. Démarrez l’instance d’auteur (si ce n’est pas déjà fait) et accédez au gestionnaire de modules à l’adresse [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Téléchargez le package [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) et installez-le à l’aide de Package Manager.

   Ce package installe une configuration qui permet à l’instance d’auteur de publier du contenu sur l’instance de publication. Les étapes manuelles de [cette configuration sont disponibles ici](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > Dans un environnement d’AEM en tant que Cloud Service, le niveau Auteur est automatiquement configuré pour distribuer le contenu au niveau Publication.

1. Dans le menu **AEM Démarrer**, accédez à **Outils** > **Ressources** > **Modèles de fragment de contenu**.

1. Cliquez dans le dossier **WKND Site** .

1. Sélectionnez les trois modèles et cliquez sur **Publier** :

   ![Publication de modèles de fragment de contenu](assets/publish-deployment/publish-contentfragment-models.png)

   Une boîte de dialogue de confirmation s’affiche. Cliquez sur **Publier**.

1. Accédez au fragment de contenu du Surf Camp de Bali à l’adresse [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Cliquez sur le bouton **Publier** dans la barre de menu supérieure.

   ![Bouton Publier dans l’éditeur de fragment de contenu](assets/publish-deployment/publish-bali-content-fragment.png)

1. L’assistant de publication affiche toutes les ressources dépendantes qui doivent être publiées. Dans ce cas, le fragment référencé **stacey-roswells** est répertorié et plusieurs images sont également référencées. Les ressources référencées sont publiées avec le fragment.

   ![Ressources référencées à publier](assets/publish-deployment/referenced-assets.png)

   Cliquez de nouveau sur le bouton **Publier** pour publier le fragment de contenu et les ressources dépendantes.

1. Revenez à l’application React s’exécutant à l’adresse [http://localhost:5000/](http://localhost:5000/). Vous pouvez maintenant cliquer sur le camp de surf de Bali pour voir les détails de l&#39;aventure.

1. Revenez à l’instance d’auteur AEM à l’adresse [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) et mettez à jour le **titre** du fragment. **Enregistrez et** fermez le fragment. Ensuite, **publiez** le fragment.
1. Revenez à [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) et observez les modifications publiées.

   ![Mise à jour de la publication du Surf Camp de Bali](assets/publish-deployment/bali-surf-camp-update.png)

## Mise à jour de la configuration des fichiers COR

AEM est sécurisé par défaut et n’autorise pas les propriétés web non-AEM à effectuer des appels côté client. AEM configuration CORS (Cross-Origin Resource Sharing) peut permettre à des domaines spécifiques d’effectuer des appels vers AEM.

Ensuite, testez la configuration CORS de l’instance de publication AEM.

1. Revenez à la fenêtre du terminal où l’application React est en cours d’exécution avec la commande `npm run serve` :

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   Notez que deux URL sont fournies. Une utilisant `localhost` et une autre utilisant l’adresse IP du réseau local.

1. Accédez à l’adresse commençant par [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). L&#39;adresse sera légèrement différente pour chaque ordinateur local. Notez qu’il existe une erreur CORS lors de la récupération des données. En effet, la configuration CORS actuelle autorise uniquement les requêtes de `localhost`.

   ![Erreur CORS](assets/publish-deployment/cors-error-not-fetched.png)

   Ensuite, mettez à jour la configuration CORS de publication AEM pour autoriser les requêtes provenant de l’adresse IP du réseau.

1. Accédez à [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) et connectez-vous avec le nom d’utilisateur `admin` et le mot de passe `admin`.

1. Accédez à [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) et recherchez la configuration WKND GraphQL à l’adresse `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Mettez à jour le champ **Origines autorisées** pour inclure l’adresse IP du réseau :

   ![Mise à jour de la configuration CORS](assets/publish-deployment/cors-update.png)

   Il est également possible d’inclure une expression régulière afin d’autoriser toutes les requêtes provenant d’un sous-domaine spécifique. Enregistrez les modifications.

1. Recherchez **Apache Sling Referrer Filter** et passez en revue la configuration. La configuration **Allow Empty** est également nécessaire pour activer les requêtes GraphQL d’un domaine externe.

   ![Filtre référent Sling](assets/publish-deployment/sling-referrer-filter.png)

   Ils ont été configurés dans le cadre du site de référence WKND. Vous pouvez afficher l’ensemble complet des configurations OSGi via [le référentiel GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > Les configurations OSGi sont gérées dans un projet AEM validé dans le contrôle de code source. Un projet AEM peut être déployé dans AEM en tant qu’environnements de Cloud Service à l’aide de Cloud Manager. L’ [archétype de projet AEM](https://github.com/adobe/aem-project-archetype) peut contribuer à générer un projet pour une mise en oeuvre spécifique.

1. Revenez à l’application React commençant par [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) et observez que l’application ne renvoie plus d’erreur CORS.

   ![Erreur CORS corrigée](assets/publish-deployment/cors-error-corrected.png)

## Félicitations ! {#congratulations}

Félicitations ! Vous avez désormais simulé un déploiement en production complet à l’aide d’un environnement de publication AEM. Vous avez également appris à utiliser la configuration CORS dans AEM.

## Autres ressources

Pour plus d’informations sur les fragments de contenu et GraphQL, voir les ressources suivantes :

* [Diffusion de contenu découplée à l’aide de fragments de contenu avec GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=fr)
* [API GraphQL d’AEM à utiliser avec des fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=fr)
* [Authentification basée sur les jetons](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=fr#authentication)
* [Déploiement du code vers AEM en tant que Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
