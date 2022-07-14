---
title: AEM Configuration rapide sans affichage à l’aide du SDK local
description: Prise en main d’Adobe Experience Manager (AEM) et de GraphQL. Installez le SDK AEM, ajoutez un exemple de contenu et déployez une application qui consomme du contenu d’AEM à l’aide de ses API GraphQL. Découvrez comment AEM alimente les expériences omnicanal.
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 64086f3f7b340b143bd281e2f6f802af07554ecf
workflow-type: tm+mt
source-wordcount: '1258'
ht-degree: 3%

---

# AEM Configuration rapide sans affichage à l’aide du SDK local {#setup}

La configuration rapide AEM sans affichage vous permet d’utiliser AEM sans affichage à l’aide du contenu de l’exemple de projet de site WKND, ainsi qu’un exemple d’application React (a SPA) qui consomme le contenu via les API GraphQL sans affichage . Ce guide utilise la méthode [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## Prérequis {#prerequisites}

Les outils suivants doivent être installés localement :

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 1. Installation du SDK AEM {#aem-sdk}

Cette configuration utilise la méthode [AEM SDK as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) pour explorer AEM API GraphQL. Cette section fournit un guide rapide pour l’installation du SDK AEM et son exécution en mode création. Un guide plus détaillé pour la configuration d’un environnement de développement local [peut être consulté ici](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> Il est également possible de suivre le tutoriel avec une [AEM environnement as a Cloud Service](./cloud-service.md). Des notes supplémentaires sur l’utilisation d’un environnement Cloud sont incluses tout au long du tutoriel.

1. Accédez au **[Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service** et télécharger la dernière version de la **AEM SDK**.

   ![Portail de distribution de logiciels](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. Décompressez le téléchargement et copiez le fichier JAR de démarrage rapide (`aem-sdk-quickstart-XXX.jar`) à un dossier dédié, c’est-à-dire `~/aem-sdk/author`.
1. Renommez le fichier jar en `aem-author-p4502.jar`.

   Le `author` name indique que le fichier Quickstart jar démarre en mode création. Le `p4502` spécifie le démarrage rapide qui s’exécute sur le port 4502.

1. Pour installer et démarrer l’instance AEM, ouvrez une invite de commande dans le dossier contenant le fichier jar, puis exécutez la commande suivante :

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. Indiquez un mot de passe administrateur en tant que `admin`. Tout mot de passe administrateur est acceptable, mais il est recommandé d’utiliser `admin` pour le développement local afin de réduire la nécessité de reconfigurer.
1. Une fois l’installation du service AEM terminée, une nouvelle fenêtre de navigateur doit s’ouvrir à l’adresse [http://localhost:4502](http://localhost:4502).
1. Connexion avec le nom d’utilisateur `admin` et le mot de passe sélectionné lors AEM démarrage initial (généralement `admin`).

## 2. Installer un exemple de contenu {#install-sample-content}

Exemple de contenu à partir du **Site de référence WKND** est utilisé pour accélérer le tutoriel. Le WKND est une marque fictive de style de vie, souvent utilisée avec une formation AEM.

Le site WKND comprend les configurations requises pour exposer une [Point d’entrée GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). Dans une mise en oeuvre concrète, suivez les étapes documentées pour [inclure les points d’entrée GraphQL ;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) dans votre projet client. A [CORS](#cors-config) a également été compilé dans le cadre du site WKND. Une configuration CORS est requise pour accorder l’accès à une application externe. Pour plus d’informations sur [CORS](#cors-config) Vous trouverez ci-dessous.

1. Téléchargez le dernier AEM compilé pour le site WKND : [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Veillez à télécharger la version standard compatible avec AEM as a Cloud Service et **not** la valeur `classic` version.

1. Dans la **AEM** , accédez à **Outils** > **Déploiement** > **Packages**.

   ![Accédez aux packages](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. Cliquez sur **Télécharger le module** et sélectionnez le package WKND téléchargé à l’étape précédente. Cliquez sur **Installer** pour installer le package.

1. Dans la **AEM** , accédez à **Ressources** > **Fichiers** > **WKND partagé** > **Anglais** > **Aventures**.

   ![Vue Dossier des aventures](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   Il s’agit d’un dossier de toutes les ressources qui composent les différentes aventures promues par la marque WKND. Cela inclut les types de médias traditionnels tels que les images et les vidéos, ainsi que les médias spécifiques aux AEM comme **Fragments de contenu**.

1. Cliquez dans le **Ski descendant Wyoming** et cliquez sur le bouton **Fragment de contenu Wyoming pour le ski en descente** carte :

   ![Carte Fragment de contenu](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. L’éditeur de fragment de contenu s’ouvre pour l’aventure du ski de descente au Wyoming.

   ![Éditeur de fragment de contenu](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   Observez que divers champs comme **Titre**, **Description**, et **Activité** définissez le fragment.

   **Fragments de contenu** sont l’une des façons de gérer le contenu dans AEM. Les fragments de contenu sont un contenu réutilisable, indépendant de la présentation, composé d’éléments de données structurés tels que du texte, du texte enrichi, des dates ou des références à d’autres fragments de contenu. Les fragments de contenu sont détaillés plus loin dans la configuration rapide.

1. Cliquez sur **Annuler** pour fermer le fragment. N’hésitez pas à naviguer dans certains des autres dossiers et à explorer les autres contenus Adventure.

>[!NOTE]
>
> Si vous utilisez un environnement de Cloud Service, consultez la documentation pour savoir comment [déployer une base de code telle que le site de référence WKND vers un environnement de Cloud Service ;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3. Téléchargez et exécutez l’application WKND React {#sample-app}

L’un des objectifs de ce tutoriel consiste à montrer comment utiliser AEM contenu d’une application externe à l’aide des API GraphQL. Ce tutoriel utilise un exemple d’application React. L’application React est intentionnellement simple et se concentre sur l’intégration avec les API GraphQL AEM.

1. Ouvrez une nouvelle invite de commande et clonez l’exemple d’application React à partir de GitHub :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Ouvrez l’application React dans `aem-guides-wknd-graphql/react-app` dans votre IDE de votre choix.
1. Dans l’IDE, ouvrez le fichier . `.env.development` at `/.env.development`. Vérifiez les `REACT_APP_AUTHORIZATION` n’est pas commentée et le fichier déclare les variables suivantes :

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   Assurez-vous que `REACT_APP_HOST_URI` pointe vers votre SDK d’AEM local. Pour des raisons pratiques, ce démarrage rapide connecte l’application React à  **Auteur AEM**. **Auteur** Les services requièrent une authentification, de sorte que l’application utilise la variable `admin` pour établir sa connexion. La connexion d’une application à l’auteur AEM est une pratique courante lors du développement, car elle facilite la itération rapide sur le contenu sans avoir à publier les modifications.

   >[!NOTE]
   >
   > Dans un scénario de production, l’application se connecte à un AEM **Publier** environnement. Cela est décrit plus en détail dans la section _Déploiement en production_ .


1. Installez et démarrez l’application React :

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. Une nouvelle fenêtre de navigateur ouvre automatiquement l’application sur [http://localhost:3000](http://localhost:3000).

   ![Application de démarrage React](assets/quick-setup/aem-sdk/react-app__home-view.png)

   Une liste du contenu de l&#39;aventure depuis AEM s&#39;affiche.

1. Cliquez sur l&#39;une des images de l&#39;aventure pour voir le détail de l&#39;aventure. Une demande est faite pour AEM de renvoyer les détails d’une aventure.

   ![Vue Détails de l’aventure](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. Utilisez les outils de développement du navigateur pour inspecter la variable **Réseau** requêtes. Afficher la variable **XHR** requêtes et observez plusieurs requêtes de GET à `/graphql/execute.json/...`. Ce préfixe de chemin appelle AEM point de terminaison de requête persistant, en sélectionnant la requête persistante à exécuter à l’aide du nom et des paramètres codés suivant le préfixe .

   ![Requête XHR du point d’entrée GraphQL](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4. Modifier le contenu dans AEM

Une fois l’application React en cours d’exécution, effectuez une mise à jour du contenu dans AEM et vérifiez que la modification est répercutée dans l’application.

1. Accédez à AEM [http://localhost:4502](http://localhost:4502).
1. Accédez à **Ressources** > **Fichiers** > **WKND partagé** > **Anglais** > **Aventures** > **[Le camp de surf de Bali](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Dossier Bali Surf Camp](assets/setup/bali-surf-camp-folder.png)

1. Cliquez dans le **Le camp de surf de Bali** fragment de contenu pour ouvrir l’éditeur de fragment de contenu.
1. Modifiez le **Titre** et le **Description** de l&#39;aventure.

   ![Modification d’un fragment de contenu](assets/setup/modify-content-fragment-bali.png)

1. Cliquez sur **Enregistrer** pour enregistrer les modifications.
1. Actualisez l’application React à l’adresse [http://localhost:3000](http://localhost:3000) pour afficher vos modifications :

   ![Bali Surf Camp Adventure mise à jour](assets/setup/overnight-bali-surf-camp-changes.png)

## 5. Explore GraphiQL {#graphiql}

1. Ouvrir [GraphiQL](http://localhost:4502/aem/graphiql.html) en accédant à **Outils** > **Général** > **Éditeur de requêtes GraphQL**
1. Sélectionnez les requêtes existantes et conservées à gauche, puis exécutez-les pour afficher les résultats.

   >[!NOTE]
   >
   > L’outil GraphiQL et l’API GraphQL sont [exploré plus en détail plus loin dans le tutoriel](../multi-step/explore-graphql-api.md).

## Félicitations !{#congratulations}

Félicitations, vous disposez désormais d’une application externe qui consomme AEM contenu avec GraphQL. N’hésitez pas à examiner le code dans l’application React et à continuer à essayer de modifier les fragments de contenu existants.

### Étapes suivantes

* [Démarrez le tutoriel AEM sans affichage](../multi-step/overview.md)
