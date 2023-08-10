---
title: Configuration CORS pour AEM GraphQL
description: Découvrez comment configurer le partage des ressources cross-origin (CORS) pour l’utiliser avec AEM GraphQL.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10830
thumbnail: KT-10830.jpg
exl-id: 394792e4-59c8-43c1-914e-a92cdfde2f8a
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: f619c431d91271b2031dcb233f3e08c3008b78ed
workflow-type: tm+mt
source-wordcount: '627'
ht-degree: 42%

---

# Partage de ressources entre origines multiples (CORS)

Le partage des ressources cross-origin (CORS) d’Adobe Experience Manager as a Cloud Service facilite les propriétés web non AEM pour effectuer des appels côté client basés sur un navigateur vers AEM API GraphQL et d’autres ressources sans affichage.

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.

## Exigence CORS

CORS est obligatoire pour les connexions par navigateur aux API GraphQL d’AEM, quand le client qui se connecte à AEM n’est PAS pris en charge à partir de la même origine (également appelée hôte ou domaine) qu’AEM.

| Type de client | [Application monopage (SPA)](../spa.md) | [Composant Web/JS](../web-component.md) | [Mobile](../mobile.md) | [Serveur à serveur](../server-to-server.md) |
|----------------------------:|:---------------------:|:-------------:|:---------:|:----------------:|
| Configuration CORS requise | ✔ | ✔ | ✘ | ✘ |

## Création AEM

L’activation de CORS sur le service AEM Author diffère des services de publication et d’aperçu d’AEM AEM. Le service AEM Author requiert une configuration OSGi à ajouter au dossier de mode d’exécution du service AEM Author et n’utilise pas de configuration de Dispatcher.

### Configuration OSGi

La configuration OSGi de CORS AEM définit les critères d’autorisation pour accepter les requêtes HTTP CORS.

| Le client se connecte à : | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration OSGi CORS | ✔ | ✘ | ✘ |


L’exemple ci-dessous définit une configuration OSGi pour l’auteur AEM (`../config.author/..`) est donc principal uniquement sur le service AEM Author.

Les principales propriétés de configuration sont :

+ `alloworigin` et/ou `alloworiginregexp` indique les origines sur lesquelles opère le client qui se connecte à AEM web.
+ `allowedpaths` indique les modèles de chemin d’URL autorisés à partir des origines spécifiées.
   + Pour prendre en charge les requêtes persistantes GraphQL d’AEM, ajoutez le motif : `/graphql/execute.json.*`
   + Pour prendre en charge les fragments d’expérience, ajoutez le motif : `/content/experience-fragments/.*`
+ `supportedmethods` spécifie les méthodes HTTP autorisées pour les requêtes CORS. Pour prendre en charge AEM requêtes persistantes GraphQL (et les fragments d’expérience), ajoutez `GET` .
+ `supportedheaders` inclut `"Authorization"` Les requêtes envoyées à l’auteur AEM doivent être autorisées.
+ `supportscredentials` est défini sur `true` comme requête à l’auteur AEM doit être autorisé.

[En savoir plus sur la configuration OSGi CORS.](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=fr)

L’exemple suivant prend en charge AEM requêtes persistantes GraphQL sur l’auteur AEM. Pour utiliser des requêtes GraphQL définies par le client, ajoutez une URL de point de terminaison GraphQL dans `allowedpaths` et `POST` à `supportedmethods`.

+ `/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~graphql.cfg.json`

```json
{
  "alloworigin":[
    "https://spa.external.com/"
  ],
  "alloworiginregexp":[
  ],
  "allowedpaths": [
    "/graphql/execute.json.*",
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

#### Exemple de configuration OSGi

+ [Vous trouverez un exemple de configuration OSGi dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json)

## Publication AEM

L’activation de CORS sur les services de publication (et d’aperçu) AEM diffère du service d’auteur AEM. Le service AEM Publish nécessite l’ajout d’une configuration du Dispatcher AEM à la configuration du Dispatcher AEM Publish. AEM Publish n’utilise pas de [Configuration OSGi](#osgi-configuration).

Lors de la configuration de CORS sur AEM Publish, assurez-vous que :

+ La variable `Origin` L’en-tête de requête HTTP ne peut pas être envoyé au service de publication AEM, en supprimant `Origin` En-tête (s’il a été ajouté) du projet AEM Dispatcher `clientheaders.any` fichier . Quelconque `Access-Control-` Les en-têtes doivent être supprimés du `clientheaders.any` et Dispatcher les gèrent, et non le service de publication AEM.
+ Si vous avez des [Configurations OSGi CORS](#osgi-configuration) activée sur votre service AEM Publish, vous devez les supprimer et migrer leurs configurations vers le [Configuration du vhost de Dispatcher](#set-cors-headers-in-vhost) décrite ci-dessous.

### Configuration du Dispatcher

Le Dispatcher du service Publication AEM (et aperçu) doit être configuré pour prendre en charge CORS.

| Le client se connecte à : | l’instance de création AEM, | Publication AEM | Prévisualisation AEM |
|-------------------------------------:|:----------:|:-------------:|:-------------:|
| Nécessite une configuration CORS de Dispatcher | ✘ | ✔ | ✔ |

#### Définition des en-têtes CORS dans vhost

1. Ouvrez le fichier de configuration vhost pour le service AEM Publish, dans votre projet de configuration de Dispatcher, généralement à l’adresse `dispatcher/src/conf.d/available_vhosts/<example>.vhost`
2. Copiez le contenu de la `<IfDefine ENABLE_CORS>...</IfDefine>` ci-dessous, dans votre fichier de configuration vhost activé.

   ```{ highlight="19"}
   <VirtualHost *:80>
     ...
     <IfModule mod_headers.c>
         ################## Start of CORS configuration ##################
   
         SetEnvIfExpr "req_novary('Origin') == ''" CORSType=none CORSProcessing=false
         SetEnvIfExpr "req_novary('Origin') != ''" CORSType=cors CORSProcessing=true CORSTrusted=false
   
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') == '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=invalidpreflight CORSProcessing=false
         SetEnvIfExpr "req_novary('Access-Control-Request-Method') != '' && %{REQUEST_METHOD} == 'OPTIONS' && req_novary('Origin') != ''" CORSType=preflight CORSProcessing=true CORSTrusted=false
         SetEnvIfExpr "req_novary('Origin') -strcmatch '%{REQUEST_SCHEME}://%{HTTP_HOST}*'" CORSType=samedomain CORSProcessing=false
   
         # For requests that require CORS processing, check if the Origin can be trusted
         SetEnvIfExpr "%{HTTP_HOST} =~ /(.*)/ " ParsedHost=$1
   
         ################## Adapt regex to match CORS origin(s) for your environment
         SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.your-domain\.tld(:\d+)?$)#" CORSTrusted=true
   
         # Extract the Origin header
         SetEnvIfNoCase ^Origin$ ^(.*)$ CORSTrustedOrigin=$1
   
         # Flush If already set
         Header unset Access-Control-Allow-Origin
         Header unset Access-Control-Allow-Credentials
   
         # Trusted
         Header always set Access-Control-Allow-Credentials "true" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Origin "%{CORSTrustedOrigin}e" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Methods "GET" "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Max-Age 1800 "expr=reqenv('CORSTrusted') == 'true'"
         Header always set Access-Control-Allow-Headers "Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers" "expr=reqenv('CORSTrusted') == 'true'"
   
         # Non-CORS or Not Trusted
         Header unset Access-Control-Allow-Credentials "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Origin "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Allow-Methods "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
         Header unset Access-Control-Max-Age "expr=reqenv('CORSProcessing') == 'false' || reqenv('CORSTrusted') == 'false'"
   
         # Always vary on origin, even if its not there.
         Header merge Vary Origin
   
         # CORS - send 204 for CORS requests which are not trusted
         RewriteCond expr "reqenv('CORSProcessing') == 'true' && reqenv('CORSTrusted') == 'false'"
         RewriteRule "^(.*)" - [R=204,L]
   
         # Remove Origin before sending to AEM Publish
         RequestHeader unset Origin
   
         ################## End of CORS configuration ##################
     </IfModule>
     ...
   </VirtualHost>
   ```

3. Faites correspondre les origines souhaitées accédant à votre service de publication AEM en mettant à jour l’expression régulière dans la ligne ci-dessous. Si plusieurs origines sont requises, dupliquez cette ligne et mettez à jour pour chaque origine/origine modèle.

   ```
   SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*.your-domain.tld(:\d+)?$)#" CORSTrusted=true
   ```

   + Par exemple, pour activer l’accès à CORS à partir des origines :

      + Tout sous-domaine sur `https://example.com`
      + Tout port sur `http://localhost`

     Remplacez la ligne par les deux lignes suivantes :

     ```
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(https://.*\.example\.com$)#" CORSTrusted=true
     SetEnvIfExpr "env('CORSProcessing') == 'true' && req_novary('Origin') =~ m#(http://localhost(:\d+)?$)#" CORSTrusted=true
     ```

#### Exemple de configuration du Dispatcher

+ [Vous trouverez un exemple de configuration de Dispatcher dans le projet WKND.](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost)
