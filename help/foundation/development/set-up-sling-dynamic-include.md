---
title: Configurer Sling Dynamic Include pour AEM
description: Présentation vidéo de l’installation et de l’utilisation d’Apache Sling Dynamic Include avec AEM Dispatcher s’exécutant sur le serveur web Apache HTTP.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 874
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 100%

---

# Configuration [!DNL Sling Dynamic Include]

Présentation vidéo de l’installation et de l’utilisation d’[!DNL Apache Sling Dynamic Include] avec [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr) s’exécutant sur [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> Assurez-vous que la dernière version d’AEM Dispatcher est installée localement.

1. Téléchargez et installez le [[!DNL Sling Dynamic Include] lot](https://sling.apache.org/downloads.cgi).
1. Configurez [!DNL Sling Dynamic Include] via la [!DNL OSGi Configuration Factory] sur **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Ou, pour ajouter à une base de code AEM, créez le nœud approprié **sling:OsgiConfig** sur :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Facultatif) Répétez la dernière étape pour autoriser les composants sur [contenu verrouillé (initial) des modèles modifiables](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html?lang=fr) à être diffusés via [!DNL SDI] également. La raison de la configuration supplémentaire est que le contenu verrouillé des modèles modifiables est diffusé à partir de `/conf` au lieu de `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. Mettez à jour le fichier `httpd.conf` de [!DNL Apache HTTPD Web server] pour activer le module [!DNL Include].

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Mettez à jour le fichier [!DNL vhost] afin de respecter les directives d’inclusion.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Mettez à jour le fichier de configuration dispatcher.any pour prendre en charge (1) les sélecteurs `nocache` et (2) activer la prise en charge TTL.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Laisser l’élément `*` à la fin de la règle `*.nocache.html*` glob ci-dessus peut entraîner des [problèmes dans les demandes de sous-ressources](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Redémarrez toujours [!DNL Apache HTTP Web Server] après avoir apporté des modifications à ses fichiers de configuration ou à `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Si vous utilisez [!DNL Sling Dynamic Includes] pour la diffusion Edge-Side Includes (ESI), assurez-vous de mettre en cache les [en-têtes de réponse pertinents dans le cache du Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=fr#CachingHTTPResponseHeaders). Les en-têtes possibles sont les suivants :
>
>* « Cache-Control »
>* « Content-Disposition »
>* « Content-Type »
>* « Date d’expiration »
>* « Last-Modified »
>* « ETag »
>* « X-Content-Type-Options »
>* « Last-Modified »
>

## Documents annexes

* [Télécharger le lot Sling Dynamic Include](https://sling.apache.org/downloads.cgi)
* [Documentation Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
