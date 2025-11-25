---
title: Chargement de ressources par programme vers AEM as a Cloud Service
description: Découvrez comment charger des ressources vers AEM as a Cloud Service à l’aide de la bibliothèque Node.js @adobe/aem-upload.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer, Architect
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: 151a5220ee842ee77ae27e99ded62f8d3dae4612
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 2%

---


# Chargement de ressources par programme vers AEM as a Cloud Service

Découvrez comment charger des ressources dans un environnement AEM as a Cloud Service à l’aide de l’application cliente qui utilise la bibliothèque Node.js [aem-upload](https://github.com/adobe/aem-upload).


>[!VIDEO](https://video.tv.adobe.com/v/3476952?quality=12&learn=on)


## Ce que vous apprenez

Dans ce tutoriel, vous apprenez ce qui suit :

+ Comment utiliser l’approche _chargement binaire direct_ pour charger des ressources dans l’environnement AEM as a Cloud Service (RDE, développement, évaluation et production) à l’aide de la bibliothèque Node.js [aem-upload](https://github.com/adobe/aem-upload).
+ Comment configurer et exécuter l’application [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) pour charger des ressources dans l’environnement AEM as a Cloud Service.
+ Consultez l’exemple de code d’application et comprenez les détails d’implémentation.
+ Découvrez les bonnes pratiques relatives au chargement de ressources par programmation dans l’environnement AEM as a Cloud Service.

## Comprendre l’approche _chargement binaire direct_

L’approche _chargement binaire direct_ permet de charger des fichiers à partir de votre système source _directement vers l’espace de stockage_ dans un environnement AEM as a Cloud Service à l’aide d’une _URL présignée_. Il n’est plus nécessaire d’acheminer les données binaires via les processus Java AEM, ce qui accélère les chargements et réduit la charge du serveur.

Avant d’exécuter l’exemple d’application, comprenons le flux de chargement binaire direct.

Dans le flux de chargement binaire direct, les données binaires sont chargées directement dans l’espace de stockage cloud avec des URL prédéfinies. AEM as a Cloud Service assure des traitements légers, tels que la génération des URL prédéfinies et la notification de la fin du chargement au service AEM Asset Compute. Le diagramme de flux logique suivant illustre le flux de chargement binaire direct.

![Flux de chargement binaire direct](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### La bibliothèque de chargement AEM

La bibliothèque Node.js [aem-upload](https://github.com/adobe/aem-upload) abstrait les détails d’implémentation de l’approche _chargement binaire direct_. Il propose deux classes pour orchestrer le processus de chargement :

+ **FileSystemUpload** - Utilisez-le lors du chargement de fichiers à partir du système de fichiers local, y compris la prise en charge des structures de répertoires
+ **DirectBinaryUpload** - Utilisez-le pour un contrôle plus précis du processus de chargement binaire, comme le chargement à partir de flux ou de tampons

>[!CAUTION]
>
>Il n’existe AUCUN équivalent de la bibliothèque [aem-upload](https://github.com/adobe/aem-upload) dans Java. L’application cliente doit être écrite dans Node.js pour utiliser l’approche _chargement binaire direct_. Pour plus d’informations, consultez la page [API et opérations Experience Manager Assets](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis).

## Exemple d’application

Utilisez l’application [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) pour découvrir le processus de chargement de ressources par programmation. L’exemple d’application illustre l’utilisation des classes `FileSystemUpload` et `DirectBinaryUpload` de la bibliothèque [aem-upload](https://github.com/adobe/aem-upload).

### Conditions préalables

Avant d’exécuter l’exemple d’application, vérifiez que vous disposez des conditions préalables suivantes :

+ Environnement de création AEM as a Cloud Service tel que l’environnement de développement rapide (RDE), l’environnement de développement, etc.
+ Node.js (dernière version du LTS)
+ Compréhension de base de Node.js et npm

>[!CAUTION]
>
> Vous ne pouvez PAS utiliser le SDK AEM as a Cloud Service (ou instance AEM locale) pour tester le processus de chargement des ressources de la programmation. Vous devez utiliser un environnement AEM as a Cloud Service tel que l’environnement de développement rapide (RDE), l’environnement de développement, etc.

### Télécharger l’exemple d’application

1. Téléchargez le fichier zip d’application [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) et extrayez-le.

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. Ouvrez le dossier extrait dans votre éditeur de code préféré.

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. À l’aide du terminal de l’éditeur de code, installez les dépendances.

   ```bash
   $ npm install
   ```

   ![Exemple d’application](./assets/programmatic-asset-upload/install-dependencies.png)

### Configuration de l’exemple d’application

Avant d’exécuter l’exemple d’application, vous devez le configurer avec les détails de l’environnement AEM as a Cloud Service nécessaires tels que l’URL de création AEM, la _méthode d’authentification_ et le chemin d’accès au dossier de ressources.

Il existe _plusieurs méthodes d’authentification_ prises en charge par la bibliothèque Node.js [aem-upload](https://github.com/adobe/aem-upload). Le tableau suivant résume les _méthodes d’authentification_ prises en charge et leur objectif.

| | Authentification de base | [&#x200B; Jeton de développement local &#x200B;](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [Informations d’identification du service](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [OAuth S2S](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [Application web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [&#x200B; SPA OAuth &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| Est-il pris en charge ? | &check; | &check; | &check; | &croix; | &croix; | &croix; |
| Objectif | Développement local | Développement local | Production | S/O | S/O | S/O |

Pour configurer l’exemple d’application, procédez comme suit :

1. Copiez le fichier `env.example` dans `.env` fichier .

   ```bash
   $ cp env.example .env
   ```

1. Ouvrez le fichier `.env` et mettez à jour la variable d’environnement `AEM_URL` avec l’URL de création AEM as a Cloud Service.

1. Choisissez la méthode d’authentification parmi les options suivantes et mettez à jour les variables d’environnement correspondantes.

>[!BEGINTABS]

>[!TAB Authentification de base]

Pour utiliser l’authentification de base, vous devez créer un utilisateur dans l’environnement AEM as a Cloud Service.

1. Connectez-vous à votre environnement AEM as a Cloud Service.

1. Accédez à **Outils** > **Sécurité** > **Utilisateurs** et cliquez sur le bouton **Créer**.

   ![Créer un utilisateur](./assets/programmatic-asset-upload/create-user.png)

1. Saisir les détails de l’utilisateur

   ![Détails utilisateur](./assets/programmatic-asset-upload/user-details.png)

1. Dans l’onglet **Groupes**, ajoutez le groupe **Utilisateurs du DAM**. Cliquez sur le bouton **Enregistrer et fermer**.

   ![Ajouter un groupe d’utilisateurs DAM](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. Mettez à jour les variables d’environnement `AEM_USERNAME` et `AEM_PASSWORD` avec le nom d’utilisateur et le mot de passe de l’utilisateur créé.

>[!TAB  Jeton de développement local ]

Pour obtenir le jeton de développement local, vous devez utiliser le Developer Console **AEM**. Le jeton généré est de type JSON Web Token (JWT) .

1. Connectez-vous à [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) et accédez à la page de détails **Environnement** souhaitée. Cliquez sur le bouton **»... »** puis sélectionnez **Developer Console**.

   ![Developer Console](./assets/programmatic-asset-upload/developer-console.png)

1. Connectez-vous à AEM Developer Console et utilisez le bouton _Nouvelle console_ pour passer à la nouvelle console.

1. Dans la section **Outils**, sélectionnez **Intégrations** et cliquez sur le bouton **Obtenir le jeton local**.

   ![Obtenir le jeton local](./assets/programmatic-asset-upload/get-local-token.png)

1. Copiez la valeur de jeton et mettez à jour la variable d’environnement `AEM_BEARER_TOKEN` avec la valeur de jeton.

Notez que le jeton de développement local est valide pendant 24 heures et est émis pour l’utilisateur ou l’utilisatrice qui a généré le jeton.

>[!TAB Informations d’identification du service]

Pour obtenir les informations d’identification de service, vous devez utiliser le Developer Console **AEM**. Il est utilisé pour générer le jeton de type JSON Web Token (JWT) à l’aide du module npm [jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth).

1. Connectez-vous à [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) et accédez à la page de détails **Environnement** souhaitée. Cliquez sur le bouton **»... »** puis sélectionnez **Developer Console**.

   ![Developer Console](./assets/programmatic-asset-upload/developer-console.png)

1. Connectez-vous à AEM Developer Console et utilisez le bouton _Nouvelle console_ pour passer à la nouvelle console.

1. Dans la section **Outils**, sélectionnez **Intégrations** et cliquez sur le bouton **Créer un compte technique**.

   ![Obtention des informations d’identification de service](./assets/programmatic-asset-upload/get-service-credentials.png)

1. Cliquez sur l’option **Affichage** pour copier le fichier JSON des informations d’identification du service.

   ![Informations d’identification du service](./assets/programmatic-asset-upload/service-credentials.png)

1. Créez un fichier `service-credentials.json` à la racine de l’exemple d’application et collez le code JSON des informations d’identification du service dans le fichier .

1. Mettez à jour la variable d’environnement `AEM_SERVICE_CREDENTIALS_FILE` avec le chemin d’accès au fichier service-credentials.json.

1. Assurez-vous que l’utilisateur des informations d’identification du service dispose des autorisations nécessaires pour charger des ressources dans l’environnement AEM as a Cloud Service. Pour plus d’informations, voir la page [Configurer l’accès dans AEM](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem).

>[!ENDTABS]

Voici un exemple complet de fichier `.env` avec les trois méthodes d’authentification configurées.

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### Exécuter l’exemple d’application

L’exemple d’application présente trois différentes manières de charger des exemples de ressources dans l’environnement AEM as a Cloud Service.

1. **FileSystemUpload** - Chargez des fichiers à partir d’un système de fichiers local avec prise en charge de la structure de répertoires et création automatique de dossiers
1. **DirectBinaryUpload** - Charge un [fichier distant](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets). Le fichier binaire est mis en mémoire tampon avant le chargement dans l’environnement AEM as a Cloud Service.
1. **Chargement par lots** - Charge plusieurs fichiers par lots à partir d’un système de fichiers local avec une logique de reprise automatique et une récupération en cas d’erreur. En arrière-plan, il utilise la classe `FileSystemUpload` pour charger des fichiers à partir du système de fichiers local.

Les ressources à charger se trouvent dans le dossier `sample-assets` et contiennent des sous-dossiers `img`, `video` et `doc` contenant chacun quelques exemples de ressources.

1. Pour exécuter l’exemple d’application, utilisez la commande suivante :

```bash
$ npm start
```

1. Saisissez l’option souhaitée _numéro_ parmi les choix suivants :

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

Les onglets suivants présentent l’exemple d’exécution d’application, sa sortie et les ressources chargées dans l’environnement AEM as a Cloud Service pour chaque méthode de chargement.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

1. Exemple de sortie d’application pour `FileSystemUpload` option :

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets téléchargé à l’aide de l’option `FileSystemUpload` dans l’environnement AEM as a Cloud Service :

   ![Ressources chargées dans un environnement AEM as a Cloud Service à l’aide de la classe FileSystemUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. Exemple de sortie d’application pour `DirectBinaryUpload` option :

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. Assets téléchargé à l’aide de l’option `DirectBinaryUpload` dans l’environnement AEM as a Cloud Service :

![Ressources chargées dans un environnement AEM as a Cloud Service à l’aide de la classe DirectBinaryUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB Chargement par lots]

1. Exemple de sortie d’application pour `Batch Upload` option :

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets téléchargé à l’aide de l’option `Batch Upload` dans l’environnement AEM as a Cloud Service :

![Ressources chargées dans un environnement AEM as a Cloud Service à l’aide de la classe BatchUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## Examiner l’exemple de code d’application

Le principal point d’entrée de l’exemple d’application est le fichier `index.js`. Il contient la fonction `promptUser` qui invite l’utilisateur à choisir et exécute l’exemple sélectionné.

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

Pour obtenir le code complet, reportez-vous au fichier `index.js` de l’exemple d’application.

Les onglets suivants présentent les détails d’implémentation de chaque méthode de chargement.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

La classe `FileSystemUpload` est utilisée pour charger des fichiers à partir du système de fichiers local avec la prise en charge de la structure de répertoires et la création automatique de dossiers.

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

Pour obtenir le code complet, reportez-vous au fichier `examples/filesystem-upload.js` de l’exemple d’application.

>[!TAB DirectBinaryUpload]

La classe `DirectBinaryUpload` est utilisée pour charger un fichier distant dans l’environnement AEM as a Cloud Service.

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

Pour obtenir le code complet, reportez-vous au fichier `examples/direct-binary-upload.js` de l’exemple d’application.

>[!TAB Chargement par lots]

Il divise les fichiers en lots et les charge par lots avec une logique de reprise automatique et une récupération des erreurs. En arrière-plan, il utilise la classe `FileSystemUpload` pour charger des fichiers à partir du système de fichiers local.

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

Pour obtenir le code complet, reportez-vous au fichier `examples/batch-upload.js` de l’exemple d’application.

>[!ENDTABS]

En outre, le fichier `README.md` de l’exemple d’application contient la documentation détaillée de l’exemple d’application.

## Bonnes pratiques

1. **Choisir la bonne méthode d’authentification :**
Utilisez les informations d’identification de service pour les environnements de production, le jeton de développement local et l’authentification de base pour le développement/test uniquement. Assurez-vous que l’utilisateur des informations d’identification du service dispose des autorisations nécessaires pour charger des ressources dans l’environnement AEM as a Cloud Service.

1. **Choisir la bonne méthode de chargement :**
Utilisez FileSystemUpload pour les fichiers locaux avec création de dossiers automatique, DirectBinaryUpload pour les flux/tampons/URL distantes avec contrôle précis et Modèle de chargement par lots pour les environnements de production avec plus de 1 000 fichiers nécessitant une logique de reprise.

1. **Structure correcte des objets de fichier DirectBinaryUpload**
Utilisez la propriété blob (et non la mémoire tampon) avec les champs obligatoires : { fileName, fileSize, blob: buffer, targetFolder } et souvenez-vous que DirectBinaryUpload ne crée PAS automatiquement de dossiers.

1. **Exemple d’application comme référence :**
L’exemple d’application est une bonne référence pour les détails d’implémentation du processus de chargement de ressources de la programmation. Vous pouvez l’utiliser comme point de départ pour votre propre mise en œuvre.
