---
title: Appeler des API AEM basées sur OpenAPI à l’aide de l’application d’une seule page OAuth
description: Découvrez comment appeler les API AEM basées sur OpenAPI sur AEM as a Cloud Service à l’aide de l’authentification des utilisateurs et utilisatrices à partir d’une application d’une seule page (SPA) personnalisée via un flux PKCE OAuth 2.0.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Intermediate
doc-type: Tutorial
jira: KT-17430
thumbnail: KT-17430.jpg
last-substantial-update: 2025-03-28T00:00:00Z
duration: 0
exl-id: 9fb92127-9dea-4a1d-b1f7-8fb98cabf188
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '2130'
ht-degree: 100%

---

# Appeler des API AEM basées sur OpenAPI à l’aide de l’application d’une seule page OAuth

Découvrez comment appeler les API AEM basées sur OpenAPI sur AEM as a Cloud Service à l’aide de l’**authentification par application d’une seule page OAuth**. Elle suit le flux PKCE (Proof Key for Code Exchange) OAuth 2.0 pour l’authentification basée sur les utilisateurs et utilisatrices dans une application d’une seule page (SPA).

L’authentification par application d’une seule page OAuth est idéale pour les applications JavaScript exécutées dans le navigateur. S’il manque un serveur principal ou si les jetons d’accès doivent être récupérés pour interagir avec les API d’AEM au nom d’un utilisateur ou d’une utilisatrice.

Le flux PKCE étend le type d’octroi OAuth 2.0 _authorization_code_, ce qui renforce la sécurité en empêchant l’interception du code d’autorisation. Pour plus d’informations, consultez la section [Différence entre les informations d’identification OAuth de serveur à serveur, d’application web et d’application d’une seule page](../overview.md#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials).

## Ce que vous apprenez{#what-you-learn}

Dans cet article, vous apprenez ce qui suit :

- Configurez un projet Adobe Developer Console (ADC) pour accéder aux API AEM basées sur OpenAPI à l’aide de l’authentification _Application d’une seule page OAuth_ ou du _flux PKCE OAuth 2.0_.

- Implémentez le flux d’authentification d’application d’une seule page OAuth dans une SPA personnalisée.
   - Authentification de l’utilisateur ou de l’utilisatrice IMS et autorisation de l’application.
   - Récupération du jeton d’accès à l’aide du flux PKCE OAuth 2.0.
   - Utilisez le jeton d’accès pour appeler les API AEM basées sur OpenAPI.

Avant de commencer, vérifiez les points suivants :

- Section [Accès aux API Adobe et concepts associés](../overview.md#accessing-adobe-apis-and-related-concepts).
- Article [Configuration des API AEM basées sur OpenAPI](../setup.md).

## Vue d’ensemble et flux fonctionnel de la SPA WKND{#wknd-spa-overview-and-functional-flow}

Examinons ce qu’est la SPA WKND, comment elle est créée et comment elle fonctionne.

La SPA WKND est une **application d’une seule page basée sur React** qui montre comment obtenir en toute sécurité un jeton d’accès spécifique à l’utilisateur ou à l’utilisatrice et interagir avec les API AEM directement du côté client. Elle implémente le flux d’authentification PKCE OAuth 2.0 via Adobe IMS et s’intègre à deux API AEM essentielles :

1. **API Sites** : pour accéder aux modèles de fragment de contenu
1. **API Assets** : pour la gestion des dossiers de gestion des ressources numériques

Le projet Adobe Developer Console (ADC) est configuré pour activer l’authentification de l’application d’une seule page OAuth, fournissant le **client_id** requis pour lancer le flux PKCE OAuth 2.0.

>[!IMPORTANT]
>
>Le projet ADC ne fournit PAS de _client_secret_. Au lieu de cela, la SPA génère un _code_verifier_ et un _code_challenge_ pour échanger en toute sécurité le code d’autorisation contre un _jeton d’accès_. Il n’est plus nécessaire de stocker un secret client côté client, ce qui renforce la sécurité.


>[!VIDEO](https://video.tv.adobe.com/v/3456966?captions=fre_fr&quality=12&learn=on)



Le diagramme suivant illustre le flux fonctionnel de la SPA WKND _obtenant un jeton d’accès spécifique à l’utilisateur ou à l’utilisatrice pour appeler les API AEM basées sur OpenAPI_ :

![Flux d’authentification SPA WKND](../assets/spa/wknd-spa-auth-flow.png)

1. La SPA lance le flux d’authentification en dirigeant l’utilisateur ou l’utilisatrice vers le système Adobe Identity Management (IMS) via une demande d’autorisation.
1. Dans le cadre de la demande d’autorisation, la SPA envoie les _client_id_, _redirect_uri_ et _code_challenge_ à IMS, en suivant le flux PKCE OAuth 2.0. La SPA génère un _code_verifier_ aléatoire, le hache à l’aide de SHA-256 et Base64 code le résultat pour créer le _code_challenge_.
1. IMS authentifie l’utilisateur ou l’utilisatrice et, une fois l’authentification réussie, émet un _authorization_code_, qui est renvoyé à la SPA via _redirect_uri_.
1. La SPA échange le code _authorization_code_ contre un _jeton d’accès_ en envoyant une requête POST au point d’entrée du jeton IMS. Elle inclut le _code_verifier_ dans la requête de validation du _code_challenge_ envoyé précédemment. Cela permet de s’assurer que la demande d’autorisation (étape 2) et la demande de jeton (étape 4) sont liées au même flux d’authentification, empêchant les attaques d’interception.
1. IMS valide le _code_verifier_ et renvoie le _jeton d’accès_ spécifique à l’utilisateur ou à l’utilisatrice.
1. La SPA inclut le _jeton d’accès_ dans les requêtes API à AEM pour authentifier et récupérer le contenu spécifique à l’utilisateur ou à l’utilisatrice.

La SPA WKND est une application basée sur [React](https://react.dev/) qui utilise le contexte [React](https://react.dev/reference/react/createContext) pour la gestion de l’état d’authentification et le [routeur React](https://reactrouter.com/home) pour la navigation.

D’autres frameworks SPA comme Angular, Vue ou Vanilla JavaScript peuvent être utilisés pour créer des SPA qui s’intègrent aux API Adobe à l’aide des approches illustrées dans ce tutoriel.

## Comment utiliser ce tutoriel{#how-to-use-this-tutorial}

Vous pouvez approcher ce tutoriel de deux manières différentes :

- [Vérifier les fragments de code de clé SPA](#review-spa-key-code-snippets) : comprenez le flux d’authentification de l’application d’une seule page OAuth et explorez les implémentations d’appels API clés dans la SPA WKND.
- [Configurer et exécuter la SPA](#setup-and-run-the-spa) : suivez les instructions détaillées pour configurer et exécuter la SPA WKND sur votre ordinateur local.

Choisissez la méthode qui vous correspond le mieux.

## Vérifier les fragments de code de clé SPA{#review-spa-key-code-snippets}

Examinons les fragments de code de clé de la SPA WKND qui montrent comment accomplir ce qui suit :

- Obtenez un jeton d’accès spécifique à l’utilisateur ou à l’utilisatrice à l’aide du flux d’authentification d’application d’une seule page OAuth.

- Appelez les API AEM basées sur OpenAPI directement du côté client.

Ces extraits vous aident à comprendre le processus d’authentification et les interactions d’API dans la SPA.

### Télécharger le code SPA{#download-the-spa-code}

1. Téléchargez le fichier zip [SPA WKND et API AEM - Application de démonstration](../assets/spa/wknd-spa-with-aemapis-demo.zip) et extrayez-le.

1. Accédez au dossier extrait et ouvrez le fichier `.env.example` dans votre éditeur de code préféré. Vérifiez les paramètres de configuration requis.

   ```plaintext
   ########################################################################
   # Adobe IMS, Adobe Developer Console (ADC), and AEM as a Cloud Service Information
   ########################################################################
   # Adobe IMS OAuth endpoints
   REACT_APP_ADOBE_IMS_AUTHORIZATION_ENDPOINT=https://ims-na1.adobelogin.com/ims/authorize/v2
   REACT_APP_ADOBE_IMS_TOKEN_ENDPOINT=https://ims-na1.adobelogin.com/ims/token/v3
   
   # Adobe Developer Console (ADC) Project's OAuth Single-Page App credential
   REACT_APP_ADC_CLIENT_ID=<ADC Project OAuth Single-Page App credential ClientID>
   REACT_APP_ADC_SCOPES=<ADC Project OAuth Single-Page App credential Scopes>
   
   # AEM Assets Information
   REACT_APP_AEM_ASSET_HOSTNAME=<AEMCS Hostname, e.g., https://author-p63947-e1502138.adobeaemcloud.com/>
   
   ################################################
   # Single Page Application Information
   ################################################
   
   # Enable HTTPS for local development
   HTTPS=true
   PORT=3001
   
   # SSL Certificate and Key for local development 
   SSL_CRT_FILE=./ssl/server.crt
   SSL_KEY_FILE=./ssl/server.key
   
   # The URL to which the user will be redirected after the OAuth flow is complete
   REACT_APP_REDIRECT_URI=https://localhost:3000/callback
   ```

   Vous devez remplacer les espaces réservés par les valeurs réelles du projet Adobe Developer Console (ADC) et de l’instance AEM as a Cloud Service Assets.

### Authentification de l’utilisateur ou de l’utilisatrice IMS et autorisation SPA{#ims-user-authentication-and-spa-authorization}

Explorons le code qui gère l’authentification des utilisateurs et utilisatrices IMS et l’autorisation SPA. Pour récupérer les modèles de fragment de contenu et les dossiers de gestion des ressources numériques, l’utilisateur ou l’utilisatrice doit s’authentifier auprès d’Adobe IMS et accorder à la SPA WKND l’autorisation d’accéder aux API AEM en son nom.

Lors de la connexion initiale, la personne est invitée à donner son consentement, ce qui permet à la SPA WKND d’accéder en toute sécurité aux ressources requises.

![Première connexion à la SPA WKND et consentement](../assets/spa/wknd-spa-first-login-consent.png)

1. Dans le fichier `src/context/IMSAuthContext.js`, la fonction `login` lance l’authentification de l’utilisateur ou de l’utilisatrice IMS et le flux d’autorisation de l’application. Cela génère des éléments `code_verifier` et `code_challenge` aléatoires pour échanger en toute sécurité le `code` contre un jeton d’accès. Le `code_verifier` est stocké dans le stockage local pour une utilisation ultérieure. Comme mentionné précédemment, la SPA ne stocke pas et n’utilise pas les `client_secret`. Elle en génère un à la volée et l’utilise en deux étapes : les demandes `authorize` et `token`.

   ```javascript
   ...
   const login = async () => {
       try {
           const codeVerifier = generateCodeVerifier();
           const codeChallenge = generateCodeChallenge(codeVerifier);
   
           localStorage.setItem(STORAGE_KEYS.CODE_VERIFIER, codeVerifier);
   
           const params = new URLSearchParams(
               getAuthParams(AUTH_METHODS.S256, codeChallenge, codeVerifier)
           );
   
           window.location.href = `${
               APP_CONFIG.adobe.ims.authorizationEndpoint //https://ims-na1.adobelogin.com/ims/authorize/v2
           }?${params.toString()}`;
       } catch (error) {
           console.error("Login initialization failed:", error);
           throw error;
       }
   };
   ...
   
   // Generate a random code verifier
   export function generateCodeVerifier() {
       const array = new Uint8Array(32);
       window.crypto.getRandomValues(array);
       const wordArray = CryptoJS.lib.WordArray.create(array);
       return base64URLEncode(wordArray);
   }
   
   // Generate code challenge using SHA-256
   export function generateCodeChallenge(codeVerifier) {
       const hash = CryptoJS.SHA256(codeVerifier);
       return base64URLEncode(hash);
   }
   
   // Get authorization URL parameters
   const getAuthParams = useCallback((method, codeChallenge, codeVerifier) => {
       const baseParams = {
           client_id: APP_CONFIG.adobe.adc.clientId, // ADC Project OAuth Single-Page App credential ClientID
           scope: APP_CONFIG.adobe.adc.scopes, // ADC Project OAuth Single-Page App credential Scopes
           response_type: "code",
           redirect_uri: APP_CONFIG.adobe.spa.redirectUri, // SPA redirect URI https://localhost:3000/callback
           code_challenge_method: method, // S256 or plain
       };
   
       return {
           ...baseParams,
           code_challenge:
               method === AUTH_METHODS.S256 ? codeChallenge : codeVerifier,
           };
   }, []);    
   ...
   ```

   Si la personne n’est pas authentifiée sur Adobe IMS, la page de connexion d’Adobe ID s’affiche et demande à la personne de s’authentifier.

   Si la personne est déjà authentifiée, elle est redirigée vers l’URI _redirect_uri_ spécifié de la SPA WKND avec un code _authorization_code_.

### Récupération du jeton d’accès à l’aide du flux PKCE OAuth 2.0{#access-token-retrieval-using-oauth-20-pkce-flow}

La SPA WKND échange en toute sécurité le code _authorization_code_ avec Adobe IMS contre un jeton d’accès spécifique à l’utilisateur ou à l’utilisatrice à l’aide des _client_id_ et _code_verifier_.

1. Dans le fichier `src/context/IMSAuthContext.js`, la fonction `exchangeCodeForToken` échange le code _authorization_code_ contre un jeton d’accès spécifique à l’utilisateur ou à l’utilisatrice.

   ```javascript
   ...
   // Handle the callback from the Adobe IMS authorization endpoint
   const handleCallback = async (code) => {
       if (authState.isProcessingCallback) return;
   
       try {
           updateAuthState({ isProcessingCallback: true });
   
           const data = await exchangeCodeForToken(code);
   
           if (data.access_token) {
               handleStorageToken(data.access_token);
               localStorage.removeItem(STORAGE_KEYS.CODE_VERIFIER);
           }
       } catch (error) {
           console.error("Error exchanging code for token:", error);
           throw error;
       } finally {
           updateAuthState({ isProcessingCallback: false });
       }
   };
   
   ...
   // Exchange the authorization code for an access token
   const exchangeCodeForToken = useCallback(async (code) => {
       const codeVerifier = localStorage.getItem(STORAGE_KEYS.CODE_VERIFIER);
   
       if (!codeVerifier) {
           throw new Error("No code verifier found");
       }
   
       //https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(APP_CONFIG.adobe.ims.tokenEndpoint, {
           method: "POST",
           headers: { "Content-Type": "application/x-www-form-urlencoded" },
           body: new URLSearchParams({
               grant_type: "authorization_code",
               client_id: APP_CONFIG.adobe.adc.clientId, // ADC Project OAuth Single-Page App credential ClientID
               code_verifier: codeVerifier, // Code verifier generated during login
               code, // Authorization code received from the IMS
               redirect_uri: `${window.location.origin}/callback`,
           }),
       });
   
       if (!response.ok) {
           throw new Error("Token request failed");
       }
   
       return response.json();
   }, []);
   
   const handleStorageToken = useCallback(
       (token) => {
           if (token) {
               localStorage.setItem(STORAGE_KEYS.ACCESS_TOKEN, token);
               updateAuthState({ isLoggedIn: true, accessToken: token });
           }
       },
       [updateAuthState]
   );
   ...
   ```

   Le jeton d’accès est stocké dans le stockage local du navigateur et utilisé dans les appels API suivants aux API AEM.

### Accès aux API AEM basées sur OpenAPI à l’aide du jeton d’accès{#accessing-openapi-based-aem-apis-using-the-access-token}

La SPA WKND utilise le jeton d’accès spécifique à l’utilisateur ou à l’utilisatrice pour appeler les modèles de fragment de contenu et les points d’entrée de l’API des dossiers de gestion des ressources numériques.

Dans le fichier `src/components/InvokeAemApis.js`, la fonction `fetchContentFragmentModels` explique comment utiliser le jeton d’accès pour appeler les API AEM basées sur OpenAPI à partir du côté client.

```javascript
    ...
  // Fetch Content Fragment Models
  const fetchContentFragmentModels = useCallback(async () => {
    try {
      updateState({ isLoading: true, error: null });
      const data = await makeApiRequest({
        endpoint: `${API_PATHS.CF_MODELS}?cursor=0&limit=10&projection=summary`,
      });
      updateState({ cfModels: data.items });
    } catch (err) {
      updateState({ error: err.message });
      console.error("Error fetching CF models:", err);
    } finally {
      updateState({ isLoading: false });
    }
  }, [makeApiRequest, updateState]);

  // Common API request helper
  const makeApiRequest = useCallback(
    async ({ endpoint, method = "GET", passAPIKey = false, body = null }) => {
    
      // Get the access token from the local storage
      const token = localStorage.getItem("adobe_ims_access_token");
      if (!token) {
        throw new Error("No access token available. Please login again.");
      }

      const headers = {
        Authorization: `Bearer ${token}`,
        "Content-Type": "application/json",
        ...(passAPIKey && { "x-api-key": APP_CONFIG.adobe.adc.clientId }),
      };

      const response = await fetch(
        `${APP_CONFIG.adobe.aem.hostname}${endpoint}`,
        {
          method,
          headers,
          ...(body && { body: JSON.stringify(body) }),
        }
      );

      if (!response.ok) {
        throw new Error(`API request failed: ${response.statusText}`);
      }

      return method === "DELETE" ? null : response.json();
    },
    []
  );
  ...
```

## Configurer et exécuter la SPA{#setup-and-run-the-spa}

Configurez et exécutez la SPA WKND sur votre ordinateur local pour comprendre le flux d’authentification de l’application d’une seule page OAuth et les appels API.

### Prérequis{#prerequisites}

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service modernisé avec les éléments suivants :
   - AEM version `2024.10.18459.20241031T210302Z` ou version ultérieure.
   - Profils de produit avec un nouveau style (si l’environnement a été créé avant novembre 2024)

  Consultez l’article [Configurer des API AEM basées sur OpenAPI](../setup.md) pour plus d’informations.

- L’exemple de projet [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) doit être déployé dessus.

- Accédez à [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started).

- Installez [Node.js](https://nodejs.org/fr/) sur votre ordinateur local pour exécuter l’exemple d’application NodeJS.

### Étapes de développement{#development-steps}

Les étapes de développement générales sont les suivantes :

1. Configurer le projet ADC
   1. Ajoutez les API Assets et Sites.
   1. Configurez les informations d’identification de l’application d’une seule page OAuth.
1. Configurer l’instance AEM
   1. Pour activer la communication de projet ADC
   1. Pour autoriser la SPA à accéder aux API AEM en configurant les paramètres CORS.
1. Configurer et exécuter la SPA WKND sur votre ordinateur local
1. Vérifier le flux de bout en bout

### Configurer le projet ADC{#configure-adc-project}

L’étape Configurer le projet ADC est _répétée_ à partir de [Configurer les API AEM basées sur OpenAPI](../setup.md). Elle est répétée pour ajouter l’API Assets et Sites et configurer sa méthode d’authentification en tant qu’application d’une seule page OAuth.

1. Ouvrez le projet souhaité à partir d’[Adobe Developer Console](https://developer.adobe.com/console/projects).

1. Pour ajouter des API AEM, cliquez sur le bouton **Ajouter une API**.

   ![Ajouter une API](../assets/spa/add-api.png)

1. Dans la boîte de dialogue _Ajouter une API_, filtrez par _Experience Cloud_ et sélectionnez la carte **Gestion de contenu AEM CS Sites** et cliquez sur **Suivant**.

   ![Ajout d’une API AEM](../assets/spa/add-aem-sites-api.png)

   >[!TIP]
   >
   >Si la **carte d’API AEM** souhaitée est désactivée et les informations _Pourquoi est-ce désactivé ?_ indiquent le message **Licence requise**, l’une des raisons peut être que vous n’avez PAS modernisé votre environnement AEM as a Cloud Service. Pour plus d’informations, consultez [Modernisation de l’environnement AEM as a Cloud Service](../setup.md#modernization-of-aem-as-a-cloud-service-environment).

1. Ensuite, dans la boîte de dialogue _Configurer l’API_, sélectionnez l’option d’authentification **Authentification de l’utilisateur ou de l’utilisatrice** et cliquez sur **Suivant**.

   ![Configurer l’API AEM](../assets/spa/configure-aem-api.png)

1. Dans la boîte de dialogue _Configurer l’API_ suivante, sélectionnez l’option d’authentification **Application d’une seule page OAuth** et cliquez sur **Suivant**.

   ![Configuration de l’application d’une seule page OAuth](../assets/spa/configure-oauth-spa.png)

1. Dans la boîte de dialogue _Configurer l’application d’une seule page OAuth_, saisissez les détails suivants, puis cliquez sur **Suivant**.
   - URI de redirection par défaut : `https://localhost:3001/callback`
   - Modèle d’URI de redirection : `https://localhost:3001/callback`

   ![Configuration de l’application d’une seule page OAuth](../assets/spa/configure-oauth-spa-details.png)

1. Vérifiez les portées disponibles et cliquez sur **Enregistrer l’API configurée**.

   ![Enregistrement de l’API configurée](../assets/spa/save-configured-api.png)

1. Répétez les étapes ci-dessus pour ajouter l’**API de création AEM Assets**.

1. Examinez l’API AEM et la configuration de l’authentification.

   ![Configuration d’API AEM](../assets/spa/aem-api-configuration.png)

   ![Configuration d’authentification](../assets/spa/authentication-configuration.png)

### Configurer l’instance AEM pour activer la communication du projet ADC{#configure-aem-instance-to-enable-adc-project-communication}

Suivez les instructions de l’article [Configurer les API AEM basées sur OpenAPI](../setup.md#configure-the-aem-instance-to-enable-adc-project-communication) pour configurer l’instance AEM afin d’activer la communication de projet ADC.

### Configuration CORS d’AEM{#aem-cors-configuration}

Le partage des ressources entre origines multiples (CORS) d’Adobe Experience Manager as a Cloud Service facilite les propriétés web non-AEM pour effectuer des appels côté client basés sur un navigateur vers les API AEM.

1. Dans le projet AEM, recherchez ou créez le fichier `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json` à partir du dossier `/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/`.

   ![Recherche du fichier de configuration CORS](../assets/spa/locate-cors-config-file.png)

1. Ajoutez la configuration suivante au fichier.

   ```json
   {
       "alloworigin":[
         ""
       ],
       "alloworiginregexp":[
         "https://localhost:.*",
         "http://localhost:.*"
       ],
       "allowedpaths": [
         "/adobe/sites/.*",
         "/graphql/execute.json.*",
         "/content/_cq_graphql/wknd-shared/endpoint.json",
         "/content/experience-fragments/.*"
       ],
       "supportedheaders": [
         "Origin",
         "Accept",
         "X-Requested-With",
         "Content-Type",
         "Access-Control-Request-Method",
         "Access-Control-Request-Headers",
         "Authorization"
       ],
       "supportedmethods":[
         "GET",
         "HEAD",
         "POST"
       ],
       "maxage:Integer": 1800,
       "supportscredentials": true,
       "exposedheaders":[ "" ]
   }
   ```

1. Validez les modifications de configuration et envoyez les modifications au référentiel Git distant auquel le pipeline Cloud Manager est connecté.

1. Déployez les modifications ci-dessus à l’aide du pipeline FullStack dans Cloud Manager.

### Configurer et exécuter la SPA{#configure-and-run-the-spa}

1. Téléchargez le fichier zip [WKND SPA &amp; AEM APIs - Demo App](../assets/spa/wknd-spa-with-aemapis-demo.zip) et extrayez-le.

1. Accédez au dossier extrait et copiez le fichier `.env.example` vers `.env`.

1. Mettez à jour le fichier `.env` avec les paramètres de configuration requis à partir du projet Adobe Developer Console (ADC) et de l’environnement AEM as a Cloud Service. Par exemple :

   ```plaintext
   ########################################################################
   # Adobe IMS, Adobe Developer Console (ADC), and AEM as a Cloud Service Information
   ########################################################################
   # Adobe IMS OAuth endpoints
   REACT_APP_ADOBE_IMS_AUTHORIZATION_ENDPOINT=https://ims-na1.adobelogin.com/ims/authorize/v2
   REACT_APP_ADOBE_IMS_TOKEN_ENDPOINT=https://ims-na1.adobelogin.com/ims/token/v3
   REACT_APP_ADOBE_IMS_USERINFO_ENDPOINT=https://ims-na1.adobelogin.com/ims/userinfo/v2
   
   # Adobe Developer Console (ADC) Project's OAuth Single-Page App credential
   REACT_APP_ADC_CLIENT_ID=ddsfs455a4a440c48c7474687c96945d
   REACT_APP_ADC_SCOPES=AdobeID,openid,aem.folders,aem.assets.author,aem.fragments.management
   
   # AEM Assets Information
   REACT_APP_AEM_ASSET_HOSTNAME=https://author-p69647-e1453424.adobeaemcloud.com/
   
   ################################################
   # Single Page Application Information
   ################################################
   
   # Enable HTTPS for local development
   HTTPS=true
   PORT=3001
   
   # SSL Certificate and Key for local development 
   SSL_CRT_FILE=./ssl/server.crt
   SSL_KEY_FILE=./ssl/server.key
   
   # The URL to which the user will be redirected after the OAuth flow is complete
   REACT_APP_REDIRECT_URI=https://localhost:3000/callback
   ```

1. Ouvrez une fenêtre de terminal et accédez au dossier extrait. Installez les dépendances requises et démarrez la SPA WKND à l’aide de la commande suivante.

   ```bash
   $ npm install
   $ npm start
   ```

### Vérifier le flux de bout en bout{#verify-the-end-to-end-flow}

1. Ouvrez un navigateur et accédez à `https://localhost:3001` pour accéder à la SPA WKND. Acceptez l’avertissement de certificat auto-signé.

   ![Accueil SPA WKND](../assets/spa/wknd-spa-home.png)

1. Cliquez sur le bouton **Connexion Adobe IMS** pour lancer le flux d’authentification de l’application d’une seule page OAuth.

1. Authentifiez-vous auprès d’Adobe IMS et autorisez la SPA WKND à accéder aux ressources en votre nom.

1. Une fois l’authentification réussie, on vous redirige vers l’itinéraire `/invoke-aem-apis` de la SPA WKND et le jeton d’accès est stocké dans l’espace de stockage local du navigateur.

   ![Appel des API AEM de la SPA WKND](../assets/spa/wknd-spa-invoke-aem-apis.png)

1. Dans l’itinéraire `https://localhost:3001/invoke-aem-apis`, cliquez sur le bouton **Récupérer des modèles de fragment de contenu** pour appeler l’API des modèles de fragment de contenu. La SPA affiche la liste des modèles de fragment de contenu.

   ![Récupération de modèles CF de la SPA WKND](../assets/spa/wknd-spa-fetch-cf-models.png)

1. De même, dans l’onglet **Assets - API de dossiers**, vous pouvez répertorier, créer et supprimer des dossiers de gestion des ressources numériques.

   ![API Assets de la SPA WKND](../assets/spa/wknd-spa-assets-api.png)

1. Dans les outils de développement du navigateur, vous pouvez examiner les requêtes et les réponses du réseau pour comprendre les appels API.

   ![Demandes réseau de la SPA WKND](../assets/spa/wknd-spa-network-requests.png)

>[!IMPORTANT]
>
>Si la personne authentifiée ne dispose pas des autorisations nécessaires pour répertorier, créer ou supprimer des ressources AEM, les appels API échouent avec une erreur 403 Interdit. Ainsi, même si la personne est authentifiée et possède un jeton d’accès IMS valide, elle ne peut pas accéder aux ressources AEM sans les autorisations requises.

### Vérifier le code de la SPA{#review-the-spa-code}

Examinons la structure générale de code et les points d’entrée principaux de la SPA WKND. La SPA est créée à l’aide du framework React et utilise l’API React Context pour l’authentification et la gestion des états.

1. Le fichier `src/App.js` est le point d’entrée principal de la SPA WKND. Le composant Application enveloppe l’ensemble de l’application et initialise le contexte `IMSAuthProvider`.

1. L’élément `src/context/IMSAuthContext.js` crée l’élément Adobe IMSAuthContext pour fournir l’état d’authentification aux composants enfant. Cela comprend les fonctions de connexion, de déconnexion et handleCallback pour lancer le flux d’authentification de l’application d’une seule page OAuth.

1. Le dossier `src/components` contient différents composants pour démontrer les appels API aux API AEM. Le composant `InvokeAemApis.js` montre comment utiliser le jeton d’accès pour appeler les API AEM.

1. Le fichier `src/config/config.js` charge les variables d’environnement à partir du fichier `.env` et les exporte en vue de les utiliser dans l’application.

1. Le fichier `src/utils/auth.js` contient des fonctions utilitaires pour générer le code_verifier et le code_challenge pour le flux PKCE OAuth 2.0.

1. Le dossier `ssl` contient le certificat autosigné et les fichiers de clé pour exécuter le proxy HTTP SSL local.

Vous pouvez développer ou intégrer la SPA existante aux API Adobe à l’aide des approches illustrées dans ce tutoriel.

## Résumé{#summary}

Dans ce tutoriel, vous avez appris à appeler les API AEM basées sur OpenAPI sur AEM as a Cloud Service à l’aide de l’authentification de l’utilisateur ou de l’utilisatrice à partir d’une application d’une seule page (SPA) via le flux PKCE OAuth 2.0.

## Ressources supplémentaires{#additional-resources}

- [API Adobe Experience Manager as a Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/)
- [Guide de mise en œuvre de l’authentification de l’utilisateur ou de l’utilisatrice](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation)
- [Autoriser la demande](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/ims#authorize-request)
- [Récupération des jetons d’accès](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/ims#fetching-access-tokens)
