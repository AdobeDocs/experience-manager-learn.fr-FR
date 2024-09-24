---
title: Pages d’erreur personnalisées
description: Découvrez comment implémenter des pages d’erreur personnalisées pour votre site web hébergé AEM as a Cloud Service.
version: Cloud Service
feature: Brand Experiences, Configuring, Developing
topic: Content Management, Development
role: Developer
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-24T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
source-git-commit: d11b07441d8c46ce9a352e4c623ddc1781b9b9be
workflow-type: tm+mt
source-wordcount: '1355'
ht-degree: 0%

---


# Pages d’erreur personnalisées

Découvrez comment implémenter des pages d’erreur personnalisées pour votre site web hébergé AEM as a Cloud Service.

Dans ce tutoriel, vous apprenez :

- Pages d’erreur par défaut
- Pages d’erreur servies depuis
   - Type de service AEM : création, publication, prévisualisation
   - Réseau de diffusion de contenu géré par Adobe
- Options de personnalisation des pages d’erreur
   - directive Apache ErrorDocument
   - ACS AEM Commons - Gestionnaire de pages d’erreur
   - Pages d’erreur CDN

## Pages d’erreur par défaut

Examinons à quel moment les pages d’erreur s’affichent, les pages d’erreur par défaut et leur provenance.

Les pages d’erreur s’affichent lorsque :

- n’existe pas (404)
- non autorisé à accéder à une page (403)
- erreur de serveur (500) en raison de problèmes de code ou de l’inaccessibilité du serveur.

AEM as a Cloud Service fournit des _pages d’erreur par défaut_ pour les scénarios ci-dessus. Il s’agit d’une page générique qui ne correspond pas à votre marque.

La page d’erreur par défaut _est diffusée_ à partir du _type de service d’AEM_ (auteur, publication, aperçu) ou du _réseau de diffusion de contenu géré par Adobe_. Pour plus d’informations, voir le tableau ci-dessous.

| Page d’erreur diffusée depuis | Détails |
|---------------------|:-----------------------:|
| Type de service AEM : création, publication, prévisualisation | Lorsque la demande de page est traitée par le type de service AEM, la page d’erreur est diffusée à partir du type de service AEM. |
| Réseau de diffusion de contenu géré par Adobe | Lorsque le réseau de diffusion de contenu géré par l’Adobe _ne peut pas atteindre le type de service AEM_ (serveur d’origine), la page d’erreur est diffusée à partir du réseau de diffusion de contenu géré par l’Adobe. **C&#39;est un événement improbable mais qui mérite d&#39;être mentionné.** |

Cependant, vous pouvez _personnaliser AEM type de service et les pages d’erreur CDN gérées par l’Adobe_ pour qu’elles correspondent à votre marque et offrir une meilleure expérience utilisateur.

## Options de personnalisation des pages d’erreur

Les options suivantes sont disponibles pour personnaliser les pages d’erreur :

| Applicable à | Nom de l’option | Description |
|---------------------|:-----------------------:|:-----------------------:|
| Types de service AEM - publication et prévisualisation | directive ErrorDocument | Utilisez la directive [ErrorDocument](https://httpd.apache.org/docs/2.4/custom-error.html) dans le fichier de configuration Apache pour spécifier le chemin d’accès à la page d’erreur personnalisée. Applicable uniquement aux types de service AEM - publication et aperçu. |
| Types de service AEM : création, publication, prévisualisation | Gestionnaire de pages d’erreur ACS AEM Commons | Utilisez le [gestionnaire de page d’erreur ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html) pour personnaliser les erreurs sur tous les types de service AEM. |
| Réseau de diffusion de contenu géré par Adobe | Pages d’erreur CDN | Utilisez les pages d’erreur CDN pour personnaliser les pages d’erreur lorsque le CDN géré par l’Adobe ne peut pas atteindre le type de service AEM (serveur d’origine). |


## Conditions préalables

Dans ce tutoriel, vous apprenez à personnaliser les pages d’erreur à l’aide de la directive _ErrorDocument_, du _gestionnaire de page d’erreur ACS AEM Commons_ et des options _Pages d’erreur CDN_. Pour suivre ce tutoriel, vous devez :

- Environnement de développement [local AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) ou AEM as a Cloud Service. L’option _CDN Error Pages_ s’applique à l’environnement AEM as a Cloud Service.

- Le [projet AEM WKND](https://github.com/adobe/aem-guides-wknd) pour personnaliser les pages d’erreur.

## Configuration

- Cloner et déployer le projet AEM WKND dans votre environnement de développement AEM local en suivant les étapes ci-dessous :

  ```
  # For local AEM development environment
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallSinglePackage -PautoInstallSinglePackagePublish
  ```

- Pour l’environnement AEM as a Cloud Service, déployez le projet AEM WKND en exécutant le [pipeline full-stack](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline), voir l’exemple [pipeline hors production](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/cloud-manager/cicd-non-production-pipeline).

- Vérifiez que le rendu des pages du site WKND est correct.

## directive Apache ErrorDocument pour personnaliser les pages d’erreur{#errordocument-directive}

Examinons comment le projet [AEM WKND](https://github.com/adobe/aem-guides-wknd) utilise la directive Apache `ErrorDocument` pour afficher les pages d’erreur personnalisées.

- Le module `ui.content.sample` contient la marque [ pages d&#39;erreur](https://github.com/adobe/aem-guides-wknd/tree/main/ui.content.sample/src/main/content/jcr_root/content/wknd/language-masters/en/errors) @ `/content/wknd/language-masters/en/errors`. Vérifiez-les dans votre environnement [local AEM](http://localhost:4502/sites.html/content/wknd/language-masters/en/errors) ou AEM as a Cloud Service `https://author-p<ID>-e<ID>.adobeaemcloud.com/ui#/aem/sites.html/content/wknd/language-masters/en/errors`.

- Le fichier `wknd.vhost` du module `dispatcher` contient :
   - La [directive ErrorDocument](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L139-L143) qui pointe vers les [pages d’erreur](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/variables/custom.vars#L7-L8) ci-dessus.
   - La valeur [DispatcherPassError](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L133) est définie sur 1 afin que Dispatcher laisse Apache gérer toutes les erreurs.

  ```
  ...
  # ErrorDocument directive in wknd.vhost file
  ErrorDocument 404 ${404_PAGE}
  ErrorDocument 500 ${500_PAGE}
  ErrorDocument 502 ${500_PAGE}
  ErrorDocument 503 ${500_PAGE}
  ErrorDocument 504 ${500_PAGE}
  
  ...
  # DispatcherPassError value in wknd.vhost file
  <IfModule disp_apache2.c>
      ...
      DispatcherPassError        1
  </IfModule>
  
  # Custom error pages path in custom.vars file
  Define 404_PAGE /content/wknd/us/en/errors/404.html
  Define 500_PAGE /content/wknd/us/en/errors/500.html
  ...
  ```

- Passez en revue les pages d’erreur personnalisées du site WKND en saisissant un nom ou un chemin de page incorrect dans votre environnement, par exemple [https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html](https://publish-p105881-e991000.adobeaemcloud.com/us/en/foo/bar.html).

## Gestionnaire de pages ACS AEM Commons-Error pour personnaliser les pages d’erreur{#acs-aem-commons-error-page-handler}

Pour personnaliser les pages d’erreur à l’aide du gestionnaire de page d’erreur ACS AEM Commons, consultez la section [Utilisation](https://adobe-consulting-services.github.io/acs-aem-commons/features/error-handler/index.html#how-to-use) .

## Pages d’erreur CDN pour personnaliser les pages d’erreur{#cdn-error-pages}

Implémentons les pages d’erreur CDN pour personnaliser les pages d’erreur lorsque le CDN géré par l’Adobe ne peut pas atteindre le type de service AEM (serveur d’origine).

>[!IMPORTANT]
>
> Notez que le réseau de diffusion de contenu géré par l’Adobe ne peut pas atteindre le type de service AEM (serveur d’origine) est un événement improbable mais qui mérite d’être planifié.


### Présentation des pages d’erreur CDN

La page d’erreur du réseau de diffusion de contenu est implémentée en tant qu’application d’une seule page (SPA) par le réseau de diffusion de contenu géré par Adobe.

Le contenu de marque spécifique à WKND doit être généré dynamiquement à l’aide du fichier JavaScript. Le fichier JavaScript doit être hébergé à un emplacement accessible au public. Les fichiers statiques suivants doivent donc être développés et hébergés dans un emplacement accessible au public :

1. **jsUrl** : URL absolue du fichier JavaScript pour effectuer le rendu du contenu de la page d’erreur en créant dynamiquement des éléments d’HTML.
1. **cssUrl** : URL absolue du fichier CSS pour appliquer un style au contenu de la page d’erreur.
1. **icoUrl** : URL absolue de la favicon.

### Développement d’une page d’erreur personnalisée

Développons le contenu de la page d’erreur de marque spécifique à WKND en tant qu’application d’une seule page (SPA).

À des fins de démonstration, utilisons [React](https://react.dev/), mais vous pouvez utiliser n’importe quelle structure ou bibliothèque JavaScript.

- Créez un projet React en exécutant la commande suivante :

  ```
  $ npx create-react-app aem-cdn-error-page
  ```

- Ouvrez le projet dans votre éditeur de code préféré et mettez à jour les fichiers suivants :

   - `src/App.js` : il s’agit du composant principal qui effectue le rendu du contenu de la page d’erreur.

     ```javascript
     import logo from "./wknd-logo.png";
     import "./App.css";
     
     function App() {
       return (
         <>
           <div className="App">
             <div className="container">
             <img src={logo} className="App-logo" alt="logo" />
             </div>
           </div>
           <div className="container">
             <div className="error-code">CDN Error Page</div>
             <h1 className="error-message">Ruh-Roh! Page Not Found</h1>
             <p className="error-description">
               We're sorry, we are unable to fetch this page!
             </p>
           </div>
         </>
       );
     }
     
     export default App;
     ```

   - `src/App.css` : appliquez un style au contenu de la page d’erreur.

     ```css
     .App {
       text-align: left;
     }
     
     .App-logo {
       height: 14vmin;
       pointer-events: none;
     }
     
     
     body {
       margin-top: 0;
       padding: 0;
       font-family: Arial, sans-serif;
       background-color: #fff;
       color: #333;
       display: flex;
       justify-content: center;
       align-items: center;
     }
     
     .container {
       text-align: letf;
       padding-top: 10px;
     }
     
     .error-code {
       font-size: 4rem;
       font-weight: bold;
       color: #ff6b6b;
     }
     
     .error-message {
       font-size: 2.5rem;
       margin-bottom: 10px;
     }
     
     .error-description {
       font-size: 1rem;
       margin-bottom: 20px;
     }
     ```

   - Ajoutez le fichier `wknd-logo.png` au dossier `src`. Copiez le [fichier](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-512.png) en tant que `wknd-logo.png`.

   - Ajoutez le fichier `favicon.ico` au dossier `public`. Copiez le [fichier](https://github.com/adobe/aem-guides-wknd/blob/main/ui.frontend/src/main/webpack/resources/images/favicons/favicon-32.png) en tant que `favicon.ico`.

   - Vérifiez le contenu de la page d’erreur du réseau de diffusion de contenu de marque WKND en exécutant le projet :

     ```
     $ npm start
     ```

     Ouvrez le navigateur et accédez à `http://localhost:3000/` pour afficher le contenu de la page d’erreur CDN.

   - Créez le projet pour générer les fichiers statiques :

     ```
     $ npm run build
     ```

     Les fichiers statiques sont générés dans le dossier `build`.


Vous pouvez également télécharger le fichier [aem-cdn-error-page.zip](./assets/aem-cdn-error-page.zip) contenant les fichiers de projet React ci-dessus.

Ensuite, hébergez les fichiers statiques ci-dessus dans un emplacement accessible au public.

### Fichiers statiques de l’hôte requis pour la page d’erreur du réseau de diffusion de contenu

hébergeons les fichiers statiques dans le stockage Azure Blob. Cependant, vous pouvez utiliser n’importe quel service d’hébergement de fichiers statiques tel que [Netlify](https://www.netlify.com/), [Vercel](https://vercel.com/) ou [AWS S3](https://aws.amazon.com/s3/).

- Suivez la documentation officielle [Stockage Azure Blob](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) pour créer un conteneur et charger les fichiers statiques.

  >[!IMPORTANT]
  >
  >Si vous utilisez d’autres services d’hébergement de fichiers statiques, suivez leur documentation pour héberger les fichiers statiques.

- Assurez-vous que les fichiers statiques sont accessibles publiquement. Les paramètres de mon compte de stockage spécifique à la démonstration WKND sont les suivants :

   - **Nom du compte de stockage** : `aemcdnerrorpageresources`
   - **Nom du conteneur** : `static-files`

  ![Stockage Azure Blob.](./assets/azure-blob-storage-settings.png)

- Dans le conteneur `static-files` supérieur, les fichiers inférieurs du dossier `build` sont chargés :

   - `error.js` : le fichier `build/static/js/main.<hash>.js` est renommé `error.js` et [publiquement accessible](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js).
   - `error.css` : le fichier `build/static/css/main.<hash>.css` est renommé `error.css` et [publiquement accessible](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css).
   - `favicon.ico` : le fichier `build/favicon.ico` est téléchargé en l’état et [publiquement accessible](https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico).

Ensuite, configurez la règle CDN (errorPages) et référencez les fichiers statiques ci-dessus.

### Configuration de la règle CDN

Configurez la règle `errorPages` CDN qui utilise les fichiers statiques ci-dessus pour effectuer le rendu du contenu de la page d’erreur CDN.

1. Ouvrez le fichier `cdn.yaml` du dossier `config` principal de votre projet AEM. Par exemple, le fichier [cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) du projet WKND.

1. Ajoutez la règle CDN suivante au fichier `cdn.yaml` :

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     # The CDN Error Page configuration. 
     # The error page is displayed when the Adobe-managed CDN is unable to reach the origin server.
     # It is implemented as a Single Page Application (SPA) and WKND branded content must be generated dynamically using the JavaScript file 
     errorPages:
       spa:
         title: Adobe AEM CDN Error Page # The title of the error page
         icoUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/favicon.ico # The PUBLIC URL of the favicon
         cssUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.css # The PUBLIC URL of the CSS file
         jsUrl: https://aemcdnerrorpageresources.blob.core.windows.net/static-files/error.js # The PUBLIC URL of the JavaScript file
   ```

1. Enregistrez, validez et poussez les modifications dans le référentiel Adobe en amont.

### Déploiement de la règle CDN

Enfin, déployez la règle CDN configurée sur l’environnement AEM as a Cloud Service à l’aide du pipeline Cloud Manager.

1. Dans Cloud Manager, accédez à la section **Pipelines** .

1. Créez un nouveau pipeline ou sélectionnez le pipeline existant qui déploie uniquement les fichiers **Config**. Pour obtenir des instructions détaillées, voir [Création d’un pipeline de configuration](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Cliquez sur le bouton **Exécuter** pour déployer la règle CDN.

![Déployer la règle CDN](./assets/deploy-cdn-rule-for-cdn-error-pages.png)

### Test des pages d’erreur du réseau CDN

Pour tester les pages d’erreur CDN, procédez comme suit :

- Ouvrez le navigateur et accédez à l’URL de l’environnement Publish, ajoutez le `cdnstatus?code=404` à l’URL, par exemple, [https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404](https://publish-p105881-e991000.adobeaemcloud.com/cdnstatus?code=404) ou accédez à l’aide de l’[URL de domaine personnalisé](https://wknd.enablementadobe.com/cdnstatus?code=404)

  ![WKND - Page d’erreur CDN](./assets/wknd-cdn-error-page.png)

- Les codes pris en charge sont : 403, 404, 406, 500 et 503.

- Vérifiez l’onglet réseau du navigateur pour voir que les fichiers statiques sont chargés à partir du stockage Azure Blob. Le document d’HTML diffusé par le réseau de diffusion de contenu géré par l’Adobe contient le contenu minimum et le fichier JavaScript crée dynamiquement le contenu de la page d’erreur de marque.

  ![Onglet Réseau de page d’erreur CDN](./assets/wknd-cdn-error-page-network-tab.png)

## Résumé

Dans ce tutoriel, vous avez appris à mettre en oeuvre des pages d’erreur personnalisées pour votre site web hébergé par AEM as a Cloud Service.

Vous avez également appris les étapes détaillées de l’option Pages d’erreur du réseau de diffusion de contenu pour personnaliser les pages d’erreur lorsque le réseau de diffusion de contenu géré par l’Adobe ne peut pas atteindre le type de service AEM (serveur d’origine).



